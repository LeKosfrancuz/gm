# How to use gm (goldmark-cli).

## Piped input with parameters

```shell
> cat file.md | gm -c jasonm23-markdown stdin
```

Here `jasonm23-markdown` is converted to `https://kpym.github.io/markdown-css/jasonm23-markdown.min.css`.

## List of the available themes

The list off all available css themre is: `air`, `github`, `jasonm23-dark`, `jasonm23-foghorn`, `jasonm23-markdown`, `jasonm23-swiss`, `markedapp-byword`, `mixu-page`, `mixu-radar`, `modest`, `retro`, `roryg-ghostwriter`, `splendor`, `thomasf-solarizedcssdark`, `thomasf-solarizedcsslight`, `witex`.

All this theme are hosted on GitHub pages of the [markdown-css](https://github.com/kpym/markdown-css) project.

## Custom HTML template

The custom HTML template can contain the following variables:

- `{{.html}}` contains the parsed html code from the markdown
- `{{.css}}` contains the css link obtained by the `--css` parameter
- `{{.title}}` contains title string obtained by the `--title` parameter

```shell
> gm --html mymodel.html README.md
```

We can use a file or a string as `--html` parameter (run in bash here):

```shell
> echo "*test*" | gm -t "Test page" -s air --html $'title: {{.title}}\ncss: {{.css}}\nhtml: {{.html}}'
title: Test page
css: https://kpym.github.io/markdown-css/air.min.css
html: <p><em>test</em></p>
```

## Use gm to produce a GitLab pages website

Here is an example of possible `.gitlab-ci.yml`:

```yaml
variables:
  DOCKER_DRIVER: overlay2 # for speed up
pages:
  image: alpine
  script:
    - wget -c https://github.com/kpym/goldmark-cli/releases/download/v0.7.0/gm_0.7.0_Linux_64bit.tar.gz -O - | tar -xz gm
    - ./gm '*.md' -o public
    - mv public/README.html public/index.html
    # add here more commands to move files to public
  artifacts:
    paths:
      - public
```