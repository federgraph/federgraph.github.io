---
layout: post
title:  "Resizing a TViewport3D"
tags: Delphi FC RG
description: "Problems and solutions when resizing the FMX viewport."
---

# TViewport3D resizing repost

The original post appeared on Blogger.
Jekyll has nice colorization of snippets, so here I will add snippets and then *repost*.

---

I have almost finished an application or two.
The two applications are using the same technology.
Both are Delphi FMX applications which use a TViewport3D as the main component which fills the whole client area of the window,
at least most of the time.

There have been problems with resizing the component.
Note that I am writing a multiplatform HD application, not the 3D application.
There is a little difference between the two.
I have always been using the HD application where the TViewport3D is a normal component on the form,
and from here on I will just say Viewport.

The first problem is obvious when you look at the Resize method.
The Viewport will by default destroy the current context and create a new one whenever the component is resized.

```pascal
procedure TViewport3D.Resize;
begin
  inherited;
  //KeepContext is a new boolean property
  if KeepContext and (Context <> nil) and (Width > 0) and (Height > 0) then
  begin
    { new resize behaviour: keep context }
    FreeAndNil(FBitmap);
    FTexture.Finalize;
    FreeAndNil(FTexture);
    //FreeAndNil(FContext);
    FTexture := TTexture.Create;
    FTexture.Style := [TTextureStyle.RenderTarget];
    ITextureAccess(FTexture).TextureScale := GetViewportScale;
    FTexture.SetSize(Round(Width * GetViewportScale), Round(Height * GetViewportScale));
    //FContext := TContextManager.CreateFromTexture(FTexture, FMultisample, True);
    FContext.UpdateFromTexture(FTexture, FMultisample, True);
    Context.SetSize(FTexture.Width, FTexture.Height);
    FBitmap := TBitmap.Create(FContext.Width, FContext.Height);
  end
  else
  begin
    { do as before: recreate context }
    FreeAndNil(FBitmap);
    FreeAndNil(FTexture);
    FreeAndNil(FContext);
    FTexture := TTexture.Create;
    FTexture.Style := [TTextureStyle.RenderTarget];
    ITextureAccess(FTexture).TextureScale := GetViewportScale;
    FTexture.SetSize(Round(Width * GetViewportScale), Round(Height * GetViewportScale));
    FContext := TContextManager.CreateFromTexture(FTexture, FMultisample, True);
    FBitmap := TBitmap.Create(FContext.Width, FContext.Height);
  end;
end;
```

First thing I did was to ensure that the context is not destroyed but kept alive when resizing.

```pascal
constructor TViewport3D.Create(AOwner: TComponent);
begin
  inherited;
  KeepContext := True;
  AutoCapture := True;
  ShowHint := True;
//  Width := 100;
//  Height := 100;
... // rest of constructor ommited.
end;
```

This has revealed a bug.
I needed to set one more buffer resource to nil.
It is in method DoResize of TDX11Context.

```pascal
procedure TDX11Context.DoFreeBuffer;
begin
  FCopyBuffer := nil; // <-- was missing
  FRenderTargetMSTex := nil;
  FSwapChain := nil;
  FRenderTargetView := nil;
  FDepthStencilTex := nil;
  FDepthStencilView := nil;
end;
```

This is where other resources are also set to nil, for example the render target.
You want to add FCopyBuffer to the list (`RSP-18850`).

Then I was looking at the number of times resize is called during a manual resize of the application window with the mouse.
I needed to handle a Windows message not handled so far and act only when resize has finished (WM_EXITSIZEMOVE, `RSP-18851`).
You will always have a hard time debugging or finally improving something related to resizing if you do not have that.

It is important that the Viewport is not client aligned and therefore not automatically resized.
Just set the size of it whenever something has finished changing.
When I say size, I mean Size.
Do not set Width followed by Height; you want to use an instance of TControlSize instead.
I will get back to alignment in a moment.

> For some time, focus on application startup.

How many times is the Viewport resized when the application starts up?
It should be one.

Turns out that Width and Height are set to a default size in the constructor (see snippet above) which triggers a resize.
I got rid of that (`RSP-17296`) and it felt much better.
My application was aligned to the client area of the parent and did set the size of the Viewport to an area greater than zero.

I found out that the second application behaved better than the first and why.
In the process, I switched alignment to none which resulted in an interesting situation.

> Width and Height of the Viewport were initially zero.

This means that the context is nil and will stay nil, until you set the size.
In my code, there was one location where I needed to test for context nil,
I did and reached the next stage.

It was in the Paint method.
If the context is nil, the bitmap is also nil.
Paint should test for a nil bitmap and exit immediately without an attempt to draw anything (`RSP-18852`).

```pascal
procedure TViewport3D.Paint;
var
  R: TRectF;
  I: Integer;
begin
  // this has been added by now (RSP was fixed.)
  if FBitmap = nil then
    Exit;
...    
end;    
```

With that in place I could start up the application with an empty Viewport, which makes a good test.
You should be able to run the application with a Viewport area of zero without a crash.
If the test passes you can go on and give it a decent size.
Then it will create a context and draw as intended.

As I have said above, it should do this only once when the application starts up.

> Use a draw counter to check.

If it will draw a few times during startup this will probably be OK.
And if find that resizing seems to work ok: this is good for you,
but keep in mind that there might still be a problem.

Back to alignment: Do not align.
Then size is not adjusted during a resize,
do this when resize has ended.

> Nice that I have an OnResizeEnd event.

```pascal
procedure TCommonCustomForm.Resize;
begin
  if Assigned(FOnResize) then
    FOnResize(Self);
end;

// property OnRsizeEnd was added to FMX.Forms.
procedure TCommonCustomForm.DoResizeEnd;
begin
  if Assigned(FOnResizeEnd) then
    FOnResizeEnd(Self);
end;
```

```pascal
unit FMX.Platform.Win;
  function WndProc

          WM_EXITSIZEMOVE:
            begin
              LForm.DoResizeEnd;
            end;

```
You will have more confidence that the application will survive wild excessive resizing by the end user
who does not seem to feel the pain.

My application uses a modified context class.
It creates and uses a special buffer resource on the GPU
and it turns out there is a show stopper problem when the context is recreated too often.

You may not see this in a normal application.
But remember:

> There is a potential problem with resizing a TViewport3D in 10.3 and before !

Hew, Hu, huh, ... , Hue! Are you scared now, of using FMX?

## Testing with OSX 64 Bit target in Delphi, version 10.3.2

My attempts to have smoother resizing was done with Berlin and Tokyo releases of Delphi.
But now in July 2019, with the OSX 64 Bit compiler available in the 10.3.2 release, I revisited my project and

- merged my changes with the latest code from Embarcadero,
- compiled for and tested on OSX,
- noticed a crash when resizing the app.

> But was able to fix it !

First, instead of letting the application crash I decided to do some logging instead of raising an exception.

```pascal
procedure TContextOpenGL.DoResize;
  // ...
  Status := glCheckFramebufferStatusEXT(GL_FRAMEBUFFER_EXT);
  if Status <> GL_FRAMEBUFFER_COMPLETE_EXT then
  begin
    log.d('in DoResize 1');
    raise_(EContext3DException.CreateResFmt(@SCannotCreateRenderBuffers, [ClassName]));
  end;
  // ...
end;
```

It was done with a quick and dirty special *instrumentation* in the unit:

````pascal
const
  WantRaise = False;

procedure raise_(e: Exception);
begin
  if WantRaise then
    raise e
  else
    log.d(e.Message);
end;
```

Then I could easily see when and where and why the app was crashing.

I will spare you most of the details, except that the call stack shows where the problem is coming from.

```
SCannotCreateRenderBuffers was raised/logged
when user is resizing the app window by dragging the corner with the mouse,
or when maximizing or restoring the app window.

OSX64 target: I see log messages in IDE messages window (Windows computer).
Render-Puffer für 'TContextOpenGL' können nicht erstellt werden. Prozess RG11 (729)

OSX32 target: I see log messages in PAServer window (Mac).
Render-Puffer für 'TContextOpenGL' können nicht erstellt werden.

Call stack below: I see this in the IDE tool window.

FMX.Context.Mac.TContextOpenGL.DoResize // <-- the problem surfaces here
FMX.Types3D.TContext3D.Resize
FMX.Types3D.TContext3D.SetSize(2098,1546) // <-- but the fix in in here
FMX.Viewport3D.TViewport3D.Resize
FMX.Controls.TControl.HandleSizeChanged
FMX.Controls.TControl.SizeChanged($60F4B10)
FMX.Types.TControlSize.DoChange
FMX.Types.TControlSize.SetSize((-1,99925327301025, 2,69504579622568e-35))
FMX.Types.TControlSize.Assign($60F4990)
FMX.Controls.TControl.SetSize($60F4990)
FrmMain.TFormMain.DoOnResizeEnd
FrmMain.TFormMain.DoOnResize
FrmMain.TFormMain.FormResize($619D2E0)
FMX.Forms.TCommonCustomForm.Resize
FMX.Forms.TCommonCustomForm.SetBounds(749,79,1049,795)
FMX.Platform.Mac.TFMXWindow.windowDidResize(Pointer($6E58BF0) as NSNotification)
FMX.Platform.Mac.TFMXWindowDelegate.windowDidResize(Pointer($6E58BF0) as NSNotification)
Macapi.ObjectiveC.DispatchToDelphi
//...
```

The `Resize` method will call the virtual abstract `DoResize` method which has an empty implementation on the Windows platform.
The implementation for GLES is also empty.
But on the OSX platform DoResize is NOT empty.
In the call stack you can see that the actual implementation in FMX.Context.Mac is called.

From looking at TContext3D.SetSize it appears questionable that Resize is called after destroying buffers and before creating new buffers.
Of course, this is abstract stuff and it might be ok, you can only find out by looking in the actual implementation.

```pascal
procedure TContext3D.SetSize(const AWidth, AHeight: Integer);
begin
  if (FWidth <> AWidth) or (FHeight <> AHeight) then
  begin
    FreeBuffer;
    FWidth := AWidth;
    FHeight := AHeight;
    if FWidth < 1 then FWidth := 1;
    if FHeight < 1 then FHeight := 1;
    Resize; // <-- This is highly questionable, I would say.
    // clear matrix state
    FCurrentStates[TContextState.cs2DScene] := False;
    FCurrentStates[TContextState.cs3DScene] := False;
    //
    CreateBuffer;
  end;
end;
```

> I did an intelligent guess, commented out the Resize method call, and voila, the problematic log messages disappeared !

I am running my application (both 32 bit and 64 bit) on OSX without a problem,
smooth-resizing is working,
and I seem to have no knowledge of any remaining bugs right now.

In versions of Delphi prior to 10.2.2 they created the exceptions but did not raise them,
and I remember that I rolled back to not raising these exceptions when they started to raise them.
But now, since I do no longer make the questionable Resize call in TContext3D, I could run with the original version of FMX.Context.Mac.

Note to myself: This is something to monitor and keep in mind,
need to watch them closely in order to be able to react and keep things working in the future.