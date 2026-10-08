Command to create a new blog post:
`hugo new posts/my-first-post.md`

Command to check the blog locally:
`hugo server -D`

Poet picture sizes are: 216 x 192
Book picture sizes are: 133 x 226

Important links:
* [picresize.com](https://picresize.com/)

Citations:
```
How to use it in a post:

Hannibal crossed the Alps in 218 BC{{< cite 1 >}}. Cannae was his masterpiece{{< cite 1 2 >}}.

### Sources
* {{< source 1 >}} Livy, *The History of Rome*, Book XXI.
* {{< source 2 >}} Polybius, *The Histories*, Book III.

What it renders:
- In the text: {{< cite 1 >}} becomes a superscript [1], and {{< cite 1 2 >}} becomes [1, 2]. Each number links to its source.
- In the list: each bullet starts with [1], [2] and so on. Clicking one jumps back to the first place that source is cited.
- The bullets stay normal Markdown * items, so you can still use italics and links inside them.
```
