---
author: admin
comments: true
date: 2017-04-03T09:38:56Z
layouts: post
link: http://chiwbaka.com/python-google-go-life/
slug: python-google-go-life
title: From Python to Google Go and Life
wordpress_id: 83
---

Now that I'm an adult, I find that doing things on the side these days is nigh unsustainable when one has to spend most of the day making a living. Besides working out almost everyday and reading articles on entrepreneurship like I used to devour articles on dating, there's no time left but to get a good 8 hours of sleep.

But recently I got a chance to study Golang. As an enthusiastic Python developer, Go shows up as a language that has the same philosophy, but just happens to be compiled, statically typed, and have better support for concurrency.


## Documentation


The documentation is incredible. You can even do the Tour of Go on localhost by simply installing the gotour package.

`go get golang.org/x/tour/gotour`

But I never learned anything from that because the Go Tour is just a museum of code snippets that show you Go's features.

As usual, the best way to learn is to implement some utility that you want for your own in Go. For this, the go doc command is incredible. For example, this is the output of 'go doc json', straight from the terminal:

    
    shinichi@ayanami ~/go/src/github.com/randomshinichi/goutil $ go doc json
    package json // import "encoding/json"
    
    Package json implements encoding and decoding of JSON as defined in RFC
    4627. The mapping between JSON and Go values is described in the
    documentation for the Marshal and Unmarshal functions.
    
    See "JSON and Go" for an introduction to this package:
    https://golang.org/doc/articles/json_and_go.html
    
    func Compact(dst *bytes.Buffer, src []byte) error
    func HTMLEscape(dst *bytes.Buffer, src []byte)
    func Indent(dst *bytes.Buffer, src []byte, prefix, indent string) error
    func Marshal(v interface{}) ([]byte, error)
    func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
    func Unmarshal(data []byte, v interface{}) error
    type Decoder struct{ ... }
    func NewDecoder(r io.Reader) *Decoder
    type Delim rune
    type Encoder struct{ ... }
    func NewEncoder(w io.Writer) *Encoder
    type InvalidUTF8Error struct{ ... }
    type InvalidUnmarshalError struct{ ... }
    type Marshaler interface{ ... }
    type MarshalerError struct{ ... }
    type Number string
    type RawMessage []byte
    type SyntaxError struct{ ... }
    type Token interface{}
    type UnmarshalFieldError struct{ ... }
    type UnmarshalTypeError struct{ ... }
    type Unmarshaler interface{ ... }
    type UnsupportedTypeError struct{ ... }
    type UnsupportedValueError struct{ ... }
    


And you can go deeper and ask for documentation on the functions and structs too:

    
    shinichi@ayanami ~/go/src/github.com/randomshinichi/goutil $ go doc json.Token
    package json // import "encoding/json"
    
    type Token interface{}
    A Token holds a value of one of these types:
    
    Delim, for the four JSON delimiters [ ] { }
    bool, for JSON booleans
    float64, for JSON numbers
    Number, for JSON numbers
    string, for JSON string literals
    nil, for JSON null
    


I only needed the internet to figure out how people usually did things in Go. For the specifics, this go doc command was incredible - never even had to leave my terminal.


## The Result


A few hours took me from Hello World to a little utility that mirrors a directory structure with empty files. Why? My photos have important information in their filenames, and I want to write scripts that mess around with said filenames. Not going to do that on my real photo collection.

    
    package main
    
    import (
        "fmt"
        "path/filepath"
        "os"
    )
    
    func mirror(path string, info os.FileInfo, err error) error {
        relpath, _ := filepath.Rel("/Volumes/Toshiba 2TB/pictures/", path)
        fmt.Println(relpath)
        if info.IsDir() {
            os.Mkdir(relpath, 0755)
        } else {
            file , err := os.Create(relpath)
            file.Close()
            if err != nil {
                fmt.Println(err)
            }
        }
        return nil
    }
    
    func main(){
        fmt.Println("goutil starting")
        filepath.Walk("/Volumes/Toshiba 2TB/pictures/Photos", mirror)   
    }




## Things I noticed


Functions in Go usually return two values, the result and an error object. To receive both into variables you need := instead of =. I ran os.Create(), and some directories would have files in them but others wouldn't, so I wanted to print the error object that os.Create() returns. However, it also returns a file object, and you can't ignore that because the go compiler complains. That was seriously frustrating but it turns out that I did need the returned file handle because I was hitting the max open files limit. Good language design I suppose.

I have to say, error checking in returned values clutters up the code. Just look at the if statements above. This would be much more poetic in Python because of exceptions, which are implicit and bubble upwards from code below. Still, it probably doesn't get much better than this in the compiled language world. Also, this can still be properly mitigated by keeping functions single purpose.

To ignore a return value, use _


## Programming Languages and Social Issues


Afterwards I read [Rob Pike's blogpost](https://commandcenter.blogspot.de/2012/06/less-is-exponentially-more.html) on why people weren't moving from C++ to Go as he had originally thought. It wasn't about "the better tool for the job", or productivity, or ease of maintenance. Simply put, C++ let you have control over everything, absolutely everything, and people who program in C++ like it that way, while Go has a garbage collector.

I get it, having control over absolutely everything, if only you knew enough about the language, is empowering.

However, I found that I really appreciate it when computers help me accomplish something and then get out of the way, like a tool. That's why I use a Mac.

The choice of programming languages is now an ideology, a philosophy of life. Which brings us to the next question:

Does the inability of Lisp to gain popularity say something about the people who use it, and their life strategy?

Apparently it does, and I quickly found some articles about it.
Rudolf Winestock's [The Lisp Curse](http://www.winestockwebdesign.com/Essays/Lisp_Curse.html) is the most plausible and well explained.
Mark Tarver's [The Bipolar Lisp Programmer](http://marktarver.com/bipolar.html) is the just pithy and poetic, and it shows you what 56 years of living can do for your experience and knowledge.

The Lisp Curse also linked to [Stanislav Datskovskiy](http://www.loper-os.org/?p=69), whose very writing radiates hatred, "I'm better than you-ness", and a sense that he really is incredibly brilliant, which does no favours for his ego. I've been there, come back to earth, and I have just this one thing to say: he probably doesn't get to fuck much.

And that was my day spent learning Google Go. In the end I guess I learned more about different walks of people than anything else.
