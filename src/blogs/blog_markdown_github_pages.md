# How to Create a Blog from Markdown files and publish it using GitHub Pages
[![](https://img.shields.io/badge/01--FEB--2021-c5c5c5.svg)]()
[![](https://img.shields.io/badge/BEGINNER-9cf.svg)]()
[![](https://img.shields.io/badge/BY:-1da1f2.svg)]()
<a href="https://twitter.com/cosmoplankton?ref_src=twsrc%5Etfw" class="twitter-follow-button" data-show-count="false">Follow @cosmoplankton</a>

We come across a large number of documentations, blogs, and technical articles which are auto generated from *Markdown* files. *Markdown* files are easy to edit, most IDEs and code editors support *Markdown* preview, and it is a valuable language to learn which is a professional asset if you are a developer, a technical writer, or a tech-blogger.
Obviously, depending on your needs you could always use a freemium blogging or long form newsletter platform e.g. [Medium](https://medium.com/), [Ghost](https://ghost.org/), [Twitter Revue](https://www.getrevue.co/), [Substack](https://substack.com/), [Wordpress](https://wordpress.com/), [Auto-generated GitHub Pages](https://stackoverflow.com/questions/4750520/git-branch-gh-pages), etc. But, what we are going to discuss here has some advantages if you are predominantly working on technical articles those involve code, most importantly testable working code.

There are multiple ways to create a blog from *Markdown* files, here we will look into how to use [mdBook](https://github.com/rust-lang/mdBook) and [GitHub Pages](https://pages.github.com/). *mdBook* is also available as a [Rust](https://www.rust-lang.org/) crate, so if you are an application developer you could embed *mdBook* into your apps. *mdBook* has a great default responsive theme with multiple color schemes and also allows you to customize the theme very easily. *mdBook* is currently being used to generate the official docs and online books for a lot of *Rust* crates, so we could expect it to have long term support from the *Rust* community.

So let's start, you could watch the hands-on video recording and then follow the detailed steps below:
<div class="youtubeVideoWrapper"><iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/wUfjmGYmWK8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>

## Environment setup
We will use *Windows Subsystem For Linux (wsl2)* with *Visual Studio Code* for our development environment and IDE, this way you could follow the steps and commands irrespective of whether you are on *Linux* or *Windows*.
- Make sure you have *wsl2* on your *Windows* system and then Install an *Ubuntu LTS (e.g. 20.04)* distribution for *wsl2*. [Steps to enable wsl2 and install a Linux distribution.](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
- Install *Visual Studio Code* on your *Windows* host. [Steps to install Code.](https://code.visualstudio.com/)

In *Windows* host system:
```sh
#  Open Windows command prompt to enable wsl2 and
#  install a Linux distribution from Windows Store (e.g. Ubuntu-20.04 LTS)
wsl --help
# List available Linux distributions
wsl --list
# Launch the Linux VM with specific user
wsl --distribution Ubuntu-20.04 --user your_username_in_wsl
```
In *Linux* VM:
```sh
# We are logged into our Linux VM, go to user home directory
cd ~
# Install Cargo and Rust.
sudo apt update && sudo apt upgrade
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
cargo --version
rustc --version
# Install mdBook and the mdBook-linkcheck (optional) plugin
cargo install mdbook
cargo install mdbook-linkcheck
mdbook --version
mdbook-linkcheck --version
```
- [Rust and Cargo installation steps using rustup.](https://www.rust-lang.org/tools/install)
- [mdBook setup and installation documentation.](https://rust-lang.github.io/mdBook/index.html)

## Repository setup
First create an empty GitHub public repository "e.g. blog-example.github.io"

In *Linux* VM:
```sh
# Make sure we are in user home
cd ~
# Create and enter a repos directory
mkdir repos && cd $_
# Create a directory for our repo
mkdir blog-example.github.io && cd $_
# Initialize git
git init
# Launch Visual Studio Code
# This will launch Code on the host machine with a remote connection to the Linux VM
code .
```

## Create the Blog
Now we will work mostly in the IDE - *Visual Studio Code.* You could keep the *Code Terminal* open or toggle its visibility using `` CTRL+` `` as per your choice.

In *Code Terminal*:
```sh
# Make sure we are in our repository directory
cd ~/repos/blog-example.github.io
# Initialize mdBook, this will create the essential files and directory structure.
mdbook init --theme
# Below command will build and serve the blog on localhost.
mdbook serve --port 3031
# Below is the output of the serve command in the Terminal. CTRL+Click on the 
# http://localhost:3031 link, this will open the blog on your host machines browser.
2021-02-01 11:31:36 [INFO] (mdbook::book): Book building has started
2021-02-01 11:31:36 [INFO] (mdbook::book): Running the html backend
2021-02-01 11:31:37 [INFO] (mdbook::cmd::serve): Serving on: http://localhost:3031
2021-02-01 11:31:37 [INFO] (warp::server): Server::run; addr=127.0.0.1:3031
2021-02-01 11:31:37 [INFO] (warp::server): listening on http://127.0.0.1:3031 
2021-02-01 11:31:37 [INFO] (mdbook::cmd::watch): Listening for changes...
# press CTRL+C to stop the server
```
* We are passing the "--theme" option so that it creates a local *theme* folder in our repository. This way to customize the theme we could edit these files, and also add new files here. We would look into some of those customizations as we move along, for details regarding all the customization options available you could refer to the [mdBook theme customization documentation.](https://rust-lang.github.io/mdBook/format/theme/index.html)

* __NOTE__: It is recommended to delete the files those you are not editing in the *theme* folder, only keep the files you edited. This way *mdBook* will always take the latest default files from its global theme directory and only the customization files from your repository's local *theme* directory.

## Create the first Blog Post
Below is the default file structure that gets created, only the important files are shown you could explore and check the documentation for the rest. Now we will modify some of these files and add new files to create and customize our first blog post.
```sh
blog-example.github.io
    |__ book    # output directory with the generated static website
    |__ src     # main source directory, our blog contents (*.md) go here
    |   |__ chapter_1.md # default generated file, rename to homepage.md
    |   |__ SUMMARY.md   # main file defining the structure of our blog
    |   |__ blogs   # new directory gets created to put all our blogs
    |       |__ our_first_example_blog.md # new file gets created for first post
    |
    |__ theme   # the local theme folder, edit or add files to customize theme
    |   |__ css
    |   |   |__ additional.css # new file we added to add some custom css
    |   |   |__ ...
    |   |__ head.hbs   # new file we added to append extra items to <html><head>
    |   |__ index.hbs  # default generated index template, entry point
    |   |__ ...
    |__ book.toml   # configuration file of our blog
    |__ ... # default generated files not shown here for clarity

```
`book.toml` and `SUMMARY.md` are the most imprtant configuration files, rest of the files could remain in their default states.

Lets edit `book.toml`:
```diff
[book]
authors = ["cosmoplankton"]
language = "en"
multilingual = false
src = "src"
title = "Example Blog"

+ [output.html]
+ additional-css = ["theme/css/additional.css",]
+ additional-js = []
+ default-theme = "dark"
+ preferred-dark-theme = "ayu"
+ no-section-label = true
+ site-url = "/"
+ cname = "blog.cosmoplankton.studio"

+ [output.html.print]
+ enable = false

+ [output.html.fold]
+ enable = true
+ level = 0
```
* We added `additional.css` where we would add our custom css.
* We changed the default theme to `dark` from `light`.
* We changed `no-section-label` to `true` as we don't want the blogs to be numbered in the sidebar.
* We disabled the `print` icon. Yu could enable if you want.
* We enabled section folding in the sidebar.
* We add the `cname` field if we wish to publish to GitHub Pages using a custom domain.
* Refer to [configuration docs](https://rust-lang.github.io/mdBook/format/config.html) for more details on all the available fields.

Lets edit `SUMMARY.md`:
```diff
# Summary

- - [Chapter 1](./chapter_1.md)
+ [Example Blog - Home](./homepage.md)
+ - [First Example Blog Post](./blogs/our_first_example_blog.md)
```
* We renamed the `chapter_1.md` file to `homepage.md`
* We added the entry for our first blog post.
* Running `mdbook build` or `mdbook serve` will create the new `blogs` directory and the `our_first_example_blog.md` file for us.


Create `head.hbs` in the `theme` directory with the below content:
```html
<!-- Content here gets appended to the <html><head> of all the pages -->
<!-- Custom HTML head: Twitter support -->
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
```

Create `additional.css` in the `theme/css` directory with the below content:
```css
/* You could add your additional css here e.g. styling
for embeded content in the Markdown (*.md) files */
.youtubeVideoWrapper {
    position: relative;
    padding-bottom: 56.25%; /* 16:9 */
    height: 0;
}
.youtubeVideoWrapper iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
```

Let's add content to our first blog post, edit `our_first_example_blog.md`:
```diff
+ # Example First Blog Post
+ Here is some text. Here is some more text. wow!

+ Here is a list of items:
+ * Item A
+ * Item B

+ Here is some code sample:
+ ```js
+ console.log('Hello, World!');
+ ```

+ Here is an emebeded youtube video:
+ <div class="youtubeVideoWrapper">
+   <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/dQw4w9WgXcQ" frameborder="0" allow="accelerometer; + autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
+ </div>
```

Let's add our first blog post to the home page, edit `homepage.md`:
```diff
+ # Example Blog - Home
+ Hello! welcome to our blog, here are the posts:
+ 
+ - [Fisrt Blog Post](/blogs/our_first_example_blog.html)

```
In future to add new blog posts you just need to add the new post to `SUMMARY.md`, run `mdbook build`, and then add content to the newly created *Markdown* file in `/blogs` directory.

## Publish to GitHub Pages
The generated blog in the `book` directory is a static website, so using GitHub Pages is not mandatory. You could publish it using any other service like [GitLab Pages](https://about.gitlab.com/), [Netlify](https://www.netlify.com/), [AWS](https://aws.amazon.com/amplify/), etc.
```sh
# Make sure we are in our repository directory
cd ~/repos/blog-example.github.io
# Check status of git repo, add new files, and commit
git status
git add -A
git commit -m "new log added to dev-source"
git branch -m main
# Add the GitHub repository as a remote and push your changes
# NOTE: you might have to setup you GitHub access token
git remote add github <HTTPS_GITHUB_URL>
git push github main
# deploy the static site to GitHub Pages
git checkout -b gh-pages
git checkout main
git worktree add /tmp/book gh-pages
git worktree list
mdbook build
rm -rf /tmp/book/*
cp -rp book/* /tmp/book/
pushd /tmp/book
git add -A
git commit 'new blog deployed'
git push github gh-pages
popd
git worktree remove /tmp/book
```
* We have two branches, `main` for development source and `gh-pages` for deployment.
* Make sure `gh-pages` is the branch for GitHub Pages in your [*Settings* at GitHub](https://docs.github.com/en/github/working-with-github-pages/getting-started-with-github-pages)
* *GitHub Pages* also allows to serve a static site from a repository sub-folder, if you want you could publish that way as well.

That is all, if you go to the published web-address available in your *GitHub* repository *Settings* section the site will be live.

> If you liked this article and are interested in more similar articles, please connect on [Twitter](https://twitter.com/intent/user?user_id=328737190) and [YouTube](https://www.youtube.com/channel/UCv1uDLx-BVMPGG8qQIaxnhA?sub_confirmation=1) for updates. Thank you!


### References and External Articles:
* [mdBook online documentation](https://github.com/rust-lang/mdBook)
* [GitHub Pages](https://pages.github.com/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Comparing WSL1 and WSL2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)
* [Responsive embeded video wrappers](https://css-tricks.com/fluid-width-video/)
* [Git worktree workflow](https://git-scm.com/docs/git-worktree)

