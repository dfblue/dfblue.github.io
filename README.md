# DFBlue website

Website runs on github pages.

# Add a publication
- Copy existing publication from `_posts` folder (copy as raw)
- Create new file with `YYYY-MM-DD-Title-Of-Post` in `_posts` directory
- Paste text and change title and date
- Add header image from Imgur (need url with `.jpg` at the end)
- Write markdown
- Save!

# Updating a publication date
- Rename the post with the correct date (also update front matter)
- Add a `redirect_from` key in the front matter that points to the original url
  - example: `redirect_from: /pub/2019-07-27-deepfacelab-tutorial/`
  - can also be a list
  - see: [jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from)
- Add a quote with an update message

# Customizing
- customize css in `/assets/css/style.scss`
- customize html in `_layouts/default.html` (and add other layouts as needed)

# Preview locally
- `gem install jekyll bundler`
- `bundle update`
- `bundle install`
- `bundle exec jekyll serve --future --livereload`