# DeepFakeBlue website

Website runs on github pages.

# Add a publication
- Copy existing publication from `_posts` folder (copy as raw)
- Create new file with `YYYY-MM-DD-Title-Of-Post` in `_posts` directory
- Paste text and change title and date
- Add header image from Imgur (need url with `.jpg` at the end)
- Write markdown
- Save!

# Customizing
- customize css in `/assets/css/style.scss`
- customize html in `_layouts/default.html` (and add other layouts as needed)

# Preview locally
- `gem install jekyll bundler`
- `bundle update`
- `bundle install`
- `bundle exec jekyll serve --livereload`