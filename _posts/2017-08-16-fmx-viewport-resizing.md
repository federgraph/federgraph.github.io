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

```
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

You will have more confidence that the application will survive wild excessive resizing by the end user
who does not seem to feel the pain.

My application uses a modified context class.
It creates and uses a special buffer resource on the GPU
and it turns out there is a show stopper problem when the context is recreated too often.

You may not see this in a normal application.
But remember:

> There is a potential problem with resizing a TViewport3D in 10.3 and before !

Hew, Hu, huh, ... , Hue! Are you scared now, of using FMX?
