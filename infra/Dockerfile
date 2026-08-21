# This site is plain static HTML/CSS/JS with no framework and no build step
# (index.html, style.css, assets/ are committed as-is), so the image is just
# nginx serving the repo's static files directly - no build stage needed.
FROM nginx:1.27-alpine

LABEL org.opencontainers.image.title="Cyfamod School OSS Landing Page" \
      org.opencontainers.image.description="Public marketing site for the open-source edition of the Cyfamod School Management System"

WORKDIR /usr/share/nginx/html

# Remove nginx's default static content before copying ours in.
RUN rm -rf ./*

# Copy custom nginx configuration
COPY infra/nginx.conf /etc/nginx/conf.d/default.conf

# Copy the static site as-is - no build artifacts to filter out, but repo
# metadata (README, .git, etc.) still shouldn't enter the runtime image,
# so this only copies the files the site actually needs.
COPY index.html style.css ./
COPY assets/ ./assets/

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://127.0.0.1/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
