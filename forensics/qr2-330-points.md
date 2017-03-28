# QR2 - 330 points

When I am not practicing my [Oboe](https://en.wikipedia.org/wiki/Oboe) for band, I have been working on a QR code generator. For some reason, some of the images are not scannable. [Here](https://raw.githubusercontent.com/EasyCTF/easyctf-2017-problems/master/qr-2/qr2.bmp) is one, can you tell me what it says?

### Solution

###### Writeup by VoidMercy from phsst

The problem mentioned that this problem is related to "OBOE" somehow. We search this up and find that OBOE can also stand for off by one error. We surmised that the off by one refers to the mask of the QR code, so we manually changed the mask of the QR code to see if it would decode. Sadly, none of the masks work. (See wikipedia for more information on masks).

Then we had an idea to find out if the mask was messed up or not. Because we know the first characters of the flag is "easyctf" we can manually encode this, and xor it with the broken QR code. This would result in the mask because applying masks uses xor and the inverse operation of xor is itself.

Here is "easyctf" encrypted:

![](https://raw.githubusercontent.com/VoidMercy/EasyCTF-Writeups-2017/master/forensics/QR2/first3chars.fw.png)

Which we xored with the original broken QR (scaled down):

Original broken QR:

![](https://raw.githubusercontent.com/VoidMercy/EasyCTF-Writeups-2017/master/forensics/QR2/small.png)

Here is the script I used:

```python
from PIL import Image

def get(rgb):
    if (rgb == (255, 255, 255)):
        return 0
    return 1

qr = Image.open("qr1.png").convert("RGB")
custommask = Image.open("sicemask.png").convert("RGB")

new = Image.new("RGB", (29, 29))

for i in range(29):
    for a in range(29):
        if (get(custommask.getpixel((i, a))) ^ get(qr.getpixel((i, a))) == 0):
            new.putpixel((i, a), (255, 255, 255))
        else:
            new.putpixel((i, a), (0, 0, 0))
new.save("test.png")
```

Then here is the result (with a bit of edit to make the real mask more visible):

![](https://raw.githubusercontent.com/VoidMercy/EasyCTF-Writeups-2017/master/forensics/QR2/actualmask.PNG)

We can see that this mask is mask #6, except moved up one pixel! So we manually construct the same mask, offset by one pixel as well:

![](https://raw.githubusercontent.com/VoidMercy/EasyCTF-Writeups-2017/master/forensics/QR2/sicemask.png)

Then we xored this mask with the original messed up QR code to undo the off by one mask.

![](https://raw.githubusercontent.com/VoidMercy/EasyCTF-Writeups-2017/master/forensics/QR2/xor1.PNG)

Afterwards, we apply the correct mask #6:

![](https://raw.githubusercontent.com/VoidMercy/EasyCTF-Writeups-2017/master/forensics/QR2/xor2.PNG)

Then, the QR is able to be decoded by any QR code reader!

## Flag

>easyctf{w0w_who_kn3w_that_Oboes_c0uld_mask_a_s3cr3t?}
