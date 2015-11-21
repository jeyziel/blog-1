+++
Categories = []
Description = ""
Tags = ["stdlib", "cli"]
date = "2015-11-01T12:15:26-07:00"
title = "Converting Black & White Images to Arbitrary Two Tone Colours with the `image` Package"
+++

If you're practically illiterate when it comes to colors and graphics (as I am), and you need to create a presentation that is not going to make the audience cringe, then there's a yak that is begging to be shaved.

The process might go something like this:

Step 1: Generate a pleasing color palette using something like [Coolers][1].

Step 2: Pick a font that isn't embarrassing.

Step 3: Find the right graphics.

Step 4: Figure out how to incorporate the graphics into the slide deck.

It's that fourth step that gets hairy.

That is, unless you've spent 10-30 hours trawling stock photography websites looking for the perfect photographs that match both your presentation topic and your color palette. In that case, then you're probably all set.

On the other hand, if what you're trying to do is incorporate simple, flat, black and white images into a color scheme that is not black and white, then it's either going to be embarrassing, or complicated.

![Generated Two Tone Graphics][2]

Designers apparently use Photoshop and/or Illustrator to make black/white images have the colors they want. I hear you can use Inkscape or the Gimp, too. There's a steep learning curve, though, not to mention the initial hassle of downloading and installing these things.

An easier solution is to write a small command-line tool to do the conversion for you, and then take a screenshot of the black and white graphic on the internet and run it through your converter.

It turns out, it's very, very easy to decode a PNG using the `image` package.

## Manipulating Pixels Using the Image Package

Read the image from the filesystem.

    r, err := os.Open("screenshot.png")
    if err != nil {
    	log.Fatal(err)
    }
    defer r.Close()

Decode it into an `image.Image`.

    img, _, err := image.Decode(r)
    if err != nil {
    	log.Fatal(err)
    }

The `img` is a type that satisfies the `image.Image` interface. It's probably an `image.RGBA` or `image.NRGBA`.

Do a type conversion to access the pixel values in the PNG.

    in := m.(*image.RGBA)

Then create a new image based on the old one:

    out := &image.RGBA{
    	Pix:    make([]uint8, len(in.Pix)),
    	Stride: in.Stride,
    	Rect:   in.Rect,
    }

Create your new background and foreground colors. You can use hex literals for this, which makes it easy to match the hex colors that online color palettes will give you.

    // #2E1C2B
    bg := color.RGBA{0x2e, 0x1c, 0x2b, 0xff}

    // #893168
    fg := color.RGBA{0x89, 0x31, 0x68, 0xff}

A naive approach to deciding whether something is light or dark is to use the R, G, and B values in the pixel and cut everything off at half-way between 0 and 255.

    isBackground := func(c color.Color) bool {
    	rgb := c.(color.RGBA)
    	return rgb.R > 127 && rgb.G > 127 && rgb.B > 127
    }

Then you can loop through and replace the existing pixels with the new values.

    max := in.Rect.Max
    for x := 0; x < max.X; x++ {
    	for y := 0; y < max.Y; y++ {
    		v := in.At(x, y)
    		if isBackground(v) {
    			out.Set(x, y, bg)
    			continue
    		}
    		out.Set(x, y, fg)
    	}
    }

Finally, write the result to a new file:

    w, _ := os.Create(*outFile)
    defer func() {
    	if err := w.Close(); err != nil {
    		log.Fatal(err)
    	}
    }()

    png.Encode(w, out)

![Generated Two Tone Graphics][3]

Here's what running the black/white image (top left) looks like when running it through the generator with a few different values.

If you don't want to write this yourself, take a look at the [twotone][4] command-line tool, which adds some convenient flags for specifying background/foreground colors on the fly, as well as input and output files.

[1]: https://coolors.co
[2]: https://raw.githubusercontent.com/kytrinyx/twotone/master/fixtures/got-want.png
[3]: https://raw.githubusercontent.com/kytrinyx/twotone/master/twotone.png
[4]: https://github.com/kytrinyx/twotone
