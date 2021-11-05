# The Abbot Blog

A blog about Abbot

## How To Image

Ideally, every blog post has a main image. Our homepage lists all our blog post titles and have a brief excerpt of the post. The main image also shows up here by using an `excerpt_image` field in the front-matter.

We also put the main image in the content of the post, usually after the first paragraph of the post.

Each image must have an `alt` tag that describes the content of the image. Ideally we also add some title text.

For example:

`![Alt text describes content of image for vision impared](https://url-to-image "Title text provides a caption to the image")`

Note that we have some JavaScript that renders the title of the image below the image below the image like so:

![Screen Shot 2021-11-05 at 3 11 38 PM](https://user-images.githubusercontent.com/19977/140584230-99e8b190-ec24-47a2-8a1a-036091e1d977.png)

The title text is a great opportunity to add some funny commentary or extra context to the image. For images that we did not create, we should use the title to provide attribution for the image.

Here's an example of how we make sure the alt tag and title text show up in the homepage excerpt.

```yaml
excerpt_image:
    url: https://user-images.githubusercontent.com/19977/139963499-5f0db4c3-a72e-4bad-8419-d1ac247c676c.jpeg
    title: Puzzle image dedicated to the public domain by the author.
    alt: Puzzle with last piece fitting into the last piece of the puzzle.
```
