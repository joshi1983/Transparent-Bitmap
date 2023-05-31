unit TransBitU;
{ created by Josh Code to explain and illustrate how to create transparent graphics

Breif Explanation:

Graphic definitions

Mask -> a monochrome(black and white, no grays) image used with logical operators
     to create a transparent image
     -> It is drawn on the background in and mode to make make the opaque areas black(0's),
     the white(1's) areas are not effected because:

     11101001(background pixel) and 0000000(mask pixel in opaque area) = 000000
     // turned black
     11101001(background pixel) and 1111111(mask pixel in transparent area) = 11101001
     // not changed

Sprite -> an image used with logical operators to create a transparent image
       -> drawn after the mask
       -> some areas are copied from the original graphic(visible areas)
       -> some areas are coloured black(all 0's) so they can be drawn on the background
       in "or" mode without affecting the background because:

       00000000(background pixel after mask is drawn) or 1011101(sprite pixel in opaque area) = 1011101
       // copies the sprite pixel

       11101001(background pixel) or 0000000(sprite pixel in transparent area) = 11101001
       // not changing the background


Background -> the image the transparent image is to be drawn on
           -> this should be stored in a location that isn't modified or there should
           be some way of always recreating the background the way it was originally or
           it could screw up the tranparent animation with several drawings of the transparent graphic
              In this program, the background is stored in a separate image and is never modified.
           The background is copied to "imgDrawn1", erasing what was there before,
           and then the mask and sprite are drawn to create the transparent graphic. 


Logical operators

and -> each binary digit must be a 1, otherwise, the resulting digit is 0

ie. 1 and 1 = 1
ie. 1 and 0 = 0
ie. 0 and 1 = 0
ie. 0 and 0 = 0
ie. in binary  ->   10110111 and 00001101 = 0000101

or -> only one digit has to be a one for the resulting digit to be 1

ie. 1 or 1 = 1
ie. 1 or 0 = 1
ie. 0 or 1 = 1
ie. 0 or 0 = 0
ie. 10110111 or 00001101 = 1011111

not-> This is only using one value.  It inverts each binary digit.

ie. not 1 = 0
ie. not 0 = 1
ie. not 10110111 = 01001000

Xor ->  eXlusive or
    -> The only way to return a digit of one is if only one of the two digits are one

ie. 1 xor 1 = 0
ie. 1 xor 0 = 1
ie. 0 xor 1 = 1
ie. 0 xor 0 = 0

ie. 10110111 or 00001101 = 1011010

}
interface

uses
  Windows, Messages, SysUtils, Classes, Graphics, Controls, Forms, Dialogs,
  StdCtrls, Spin, ExtCtrls;

type
  TForm1 = class(TForm)
    ImgBackground1: TImage;
    ImgTransparent1: TImage;
    SpinEdit1: TSpinEdit;
    SpinEdit2: TSpinEdit;
    Label1: TLabel;
    Label2: TLabel;
    ImgSprite1: TImage;
    ImgMask1: TImage;
    Label3: TLabel;
    Label4: TLabel;
    Label5: TLabel;
    Label6: TLabel;
    imgDrawn1: TImage;
    Label7: TLabel;
    Button1: TButton;
    procedure SpinEdit1Change(Sender: TObject);
    procedure FormCreate(Sender: TObject);
    procedure Button1Click(Sender: TObject);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

var
  Form1: TForm1;
   initialized: boolean; // used to indicate when the sprite and mask are made
implementation

{$R *.DFM}
procedure initialize;
const TranspColour=clBlack; // the colour of pixels that will be made invisible
begin // creates the mask and sprite
     with form1 do
     begin
          imgMask1.Height:=imgTransparent1.Height;
          imgMask1.Width:=imgTransparent1.Width;
          imgSprite1.Height:=imgTransparent1.Height;
          imgSprite1.Width:=imgTransparent1.Width;
          // now the images have their dimensions equal

          // Now, start creating the Mask
          imgMask1.Canvas.Draw(0,0,imgTransparent1.Picture.Bitmap);// copy the image
          imgMask1.Picture.Bitmap.Mask(TranspColour);
          // The mask is now created.

          //Now, start creating the sprite


          imgSprite1.Canvas.Draw(0,0,imgMask1.Picture.Bitmap);
          imgSprite1.Canvas.Pen.Mode:=pmnot; // not mode for inverting the colours of the mask
          imgSprite1.Canvas.Rectangle(-1,-1,imgSprite1.Width+1,imgSprite1.Height+1);
          // invert colours of the mask
          imgSprite1.Canvas.CopyMode:=cmSrcAnd; // and mode
          imgSprite1.Canvas.Draw(0,0,imgTransparent1.Picture.Bitmap);
          // Sprite is now created.

          imgDrawn1.Height:=imgBackground1.Height;
          imgDrawn1.Width:=imgBackground1.Width;
          // the background now has the dimensions of the background
     end;
     initialized:=true;
     // indicate that the mask, and sprite has been created
end;

procedure TForm1.SpinEdit1Change(Sender: TObject);
// Draw the transparent bitmap
var
  x,y: integer; // top,left coordinates of the transparent graphic
begin
     if not initialized then
        initialize;       // Create the mask and sprite, if hasn't been done yet.
     x:=spinedit1.value;
     y:=spinedit2.value; // get the input coordinates

     imgDrawn1.Canvas.Draw(0,0,imgBackground1.Picture.Bitmap); // copy the background into the image
     imgDrawn1.Canvas.CopyMode:=cmSrcAnd; // and mode
     imgDrawn1.Canvas.Draw(x,y,imgMask1.Picture.Bitmap);
     // Mask drawn

     imgDrawn1.Canvas.CopyMode:=cmSrcPaint; // or mode
     imgDrawn1.Canvas.Draw(x,y,imgSprite1.Picture.Bitmap);


     imgDrawn1.Canvas.CopyMode:=cmSrcCopy; // the normal mode

end;

procedure TForm1.FormCreate(Sender: TObject);
begin
     initialized:=false;
end;

procedure TForm1.Button1Click(Sender: TObject);
begin // initialize
     initialize;
end;

end.
