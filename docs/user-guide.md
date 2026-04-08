# CollectionBuilder User Guide

A practical guide for digital humanities scholars to manage their digital collection using CollectionBuilder-CSV.

---

## 1. Adding Your Data via CSV

### Overview

CollectionBuilder uses a CSV file in the `_data/` directory to define your collection items. The default metadata file is `demo-metadata.csv`, but you can create your own.

### Setting Up Your Metadata File

1. **Create or edit your CSV file** in the `_data/` directory
2. **Update `_config.yml`** to point to your metadata file:

```yaml
metadata: your-filename  # without the .csv extension
```

### Required Fields

| Field | Description |
|-------|-------------|
| `objectid` | Unique identifier (lowercase, no spaces). Used in item URLs. |
| `title` | Item title displayed throughout the site. |

### Optional but Commonly Used Fields

| Field | Purpose |
|-------|---------|
| `creator` | Author/creator of the item |
| `date` | Date in YYYY, YYYY-MM, or YYYY-MM-DD format |
| `description` | Item description |
| `subject` | Subjects (semicolon-separated) |
| `location` | Geographic location |
| `latitude` / `longitude` | Coordinates for map display |
| `format` | MIME type (e.g., `image/jpeg`, `application/pdf`) |
| `display_template` | Sets item page type: `image`, `pdf`, `video`, `audio`, `record`, `item` |
| `rightsstatement` | Link to rights statement (e.g., from rightsstatements.org) |

### Example CSV Row

```csv
objectid,title,creator,date,description,subject,location,format,display_template,object_location
my_item_001,"Letter from John Smith","John Smith",1920-05-15,"Handwritten letter regarding local history",letters; correspondence,"Boston, MA",text/plain,record,/objects/my_item_001.txt
```

### Tips

- Use semicolons (`;`) to separate multiple values in fields like `subject`
- Keep `objectid` short and simple (lowercase letters, numbers, underscores)
- Test your CSV with a small sample before adding hundreds of items

---

## 2. Adding Objects (Images, PDFs, Audio, Video)

### Where to Store Files

Place all digital objects in the `objects/` folder in your project root:

```
your-collection/
├── objects/
│   ├── my_image.jpg
│   ├── document.pdf
│   └── audio.mp3
```

### Referencing Objects in Metadata

In your CSV, use **relative paths** starting with `/objects/`:

| Field | Example Value |
|-------|---------------|
| `object_location` | `/objects/my_image.jpg` (full file for download) |
| `image_small` | `/objects/small/my_image_sm.jpg` (small display version) |
| `image_thumb` | `/objects/thumbs/my_image_th.jpg` (thumbnail for browse views) |

### Supported Formats

| Type | Display Template | Common Formats |
|------|-----------------|----------------|
| Images | `image` | JPEG, PNG, TIFF |
| PDFs | `pdf` | application/pdf |
| Audio | `audio` | mp3, wav |
| Video | `video` | mp4, webm |
| External Video | `video` | YouTube/Vimeo URLs |
| Records | `record` | Metadata-only with external link |

### External Hosting

You can also reference objects hosted externally (e.g., Internet Archive, institutional repositories):

```csv
object_location: https://archive.org/download/my-item/my_file.pdf
image_small: https://archive.org/download/my-item/my_file_thumb.jpg
```

### Generating Thumbnails

If you need smaller versions of images, run:

```bash
rake generate_derivatives
```

This creates `small/` and `thumbs/` directories with resized images.

---

## 3. Adjusting the Look and Feel

### Site Configuration

Edit `_config.yml` to change basic site settings:

```yaml
title: "Your Collection Title"
tagline: "A brief description"
description: "Longer description for search engines"
url: "https://yourdomain.com"
baseurl: "/your-repo-name"
```

### Color Theme

1. Open `_data/config-theme-colors.csv`
2. Edit the `color_class` and `color` columns:

| color_class | color |
|-------------|-------|
| primary | #4232a8 |
| secondary | #6c757d |
| success | #28a745 |

This generates custom button, text, and background classes (e.g., `btn-primary`, `text-success`).

### Navigation

Edit `_data/config-nav.csv` to customize the navbar:

| menu | link | dropdown_parent |
|------|------|-----------------|
| Browse | /browse | |
| Map | /map | |
| Timeline | /timeline | |
| About | /about | |

### Custom Pages

CollectionBuilder includes built-in pages:
- `browse.html` - Gallery view of all items
- `map.html` - Geographic visualization
- `timeline.html` - Temporal visualization
- `subjects.html` - Subject browse
- `search.html` - Full-text search

Enable/disable pages by creating/removing them in the `pages/` folder.

### Further Customization

- **CSS**: Edit `assets/css/custom.css`
- **Layouts**: Modify templates in `_layouts/`
- **Includes**: Adjust components in `_includes/`

See `_includes/cb/feature_options.md` for a complete list of configuration options.

---

## Quick Reference

| Task | File to Edit |
|------|---------------|
| Add collection items | `_data/your-metadata.csv` |
| Change site title | `_config.yml` |
| Customize colors | `_data/config-theme-colors.csv` |
| Modify navigation | `_data/config-nav.csv` |
| Add custom CSS | `assets/css/custom.css` |

---

## Next Steps

1. Start with a small test collection (5-10 items)
2. Run `bundle exec jekyll serve` to preview locally
3. Push to GitHub and enable GitHub Pages
4. Iterate and expand as needed