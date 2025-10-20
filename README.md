# PeerLLM Blog

Official blog for PeerLLM - sharing insights, updates, and discussions about peer-to-peer large language models and distributed AI systems.

🌐 **Live Site**: [https://blog.peerllm.com](https://blog.peerllm.com)

## About

This blog is built with [Jekyll](https://jekyllrb.com/) and uses the [Minima](https://github.com/jekyll/minima) theme. It's automatically deployed to GitHub Pages.

## Local Development

### Prerequisites

- Ruby 3.2 or higher
- Bundler

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/PeerLLM/peerllm.github.io.git
   cd peerllm.github.io
   ```

2. Install dependencies:
   ```bash
   bundle install
   ```

3. Serve the site locally:
   ```bash
   bundle exec jekyll serve
   ```

4. Open your browser to [http://localhost:4000](http://localhost:4000)

## Creating a New Blog Post

1. Create a new file in the `_posts` directory with the naming format: `YYYY-MM-DD-title.md`

2. Add front matter at the top of the file:
   ```markdown
   ---
   layout: post
   title:  "Your Post Title"
   date:   YYYY-MM-DD HH:MM:SS +0000
   categories: your-category
   ---
   
   Your content here...
   ```

3. Write your content in Markdown format

4. Test locally, then commit and push to deploy

## Project Structure

```
.
├── _config.yml          # Jekyll configuration
├── _posts/              # Blog posts
├── _site/               # Generated site (ignored by git)
├── about.md             # About page
├── index.md             # Home page
├── Gemfile              # Ruby dependencies
└── .github/workflows/   # GitHub Actions for deployment
```

## Deployment

The site is automatically deployed to GitHub Pages when changes are pushed to the `main` branch via GitHub Actions.

## License

Content is available under [MIT License](LICENSE).
