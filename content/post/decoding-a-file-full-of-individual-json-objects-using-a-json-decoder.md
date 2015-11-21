+++
Categories = []
Description = ""
Tags = ["json"]
date = "2015-10-11T12:03:14-07:00"
title = "Decoding a File Full of Individual JSON Objects using a json.Decoder"
+++

You can wish for a proper JSON array all you like, it's not going to turn your file full of JSON objects into valid JSON. It's not that each bit of JSON isn't valid, it totally is. It's just that you've got hundreds of these JSON objects that were all dumped into the same file.

If you're new to Go, you might find yourself hunting through the `io` and `ioutil` packages for a way to read a file full of JSON objects and turn them into Go structs, but these packages will only get you partway there.

To turn a file full of distinct JSON objects into distinct Go struct values, you need two things:

1. an `io.Reader` that doesn't slurp the entire file into memory at once, and
2. a `json.Decoder`, which will let you keep reading json objects until you reach the end of the file

Here's an example file, `elements.json`, that contains a list of JSON objects. Notice that there's not a square bracket in sight. In other words, this is not an array.

    {"symbol":"S","name":"Sulfur"}
    {"symbol":"Cl","name":"Chlorine"}
    {"symbol":"K","name":"Potassium"}
    {"symbol":"Ar","name":"Argon"}
    {"symbol":"Ca","name":"Calcium"}
    {"symbol":"Sc","name":"Scandium"}
    {"symbol":"Ti","name":"Titanium"}
    ...

The objects don't have to be separated by newlines, it would still work if the objects were all jammed together on a single line.

    {"symbol":"S","name":"Sulfur"}{"symbol":"Cl","name":"Chlorine"}{"symbol":"K","name":"Potassium"}...

To get started, create a struct definition that you can unmarshal each object into.

    type Element struct {
    	Symbol string
    	Name   string
    }

Get a reader for the file using `os.Open`.

    f, err := os.Open("elements.json")
    if err != nil {
    	log.Fatal(err)
    }
    defer f.Close()

Create a decoder that reads from the open file:

    dec := json.NewDecoder(f)

Then loop until the stream reaches the end of the file:

    elements := []Element{}
    for {
    	var e Element
    	if err := dec.Decode(&amp;e); err != nil {
    		if err == io.EOF {
    			break
    		}
    		log.Fatal(err)
    	}

    	elements = append(elements, e)
    }

You can also go play with the documentation for the encoding/json package in the standard library, which [includes a runnable example][1].

[1]: https://golang.org/pkg/encoding/json/#example_Decoder
