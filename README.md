# mvi-file-attachment
A gated content plugin to handle adding files to Wordpress posts, capturing user information, and emailing links to users automatically. Supports Mailchimp. Built using MetaBox.

## Requirements
- PHP 7.4.30+
- MetaBox 5.6.7+
  - MB Admin Columns 1.6.2+
  - MB Custom Table 2.1.3+
  - MB Frontend Submission 4.1.1+
  - MB Settings Page 2.1.7+
  - Meta Box Columns 1.2.15+
  - Meta Box Tooltip 1.1.6+

## Features
- Submissions are stored in a custom table.
- Download file paths can be updated without breaking download links that were already generated.
- If a post has a file attached, then the term `has_file` will automatically be applied in the taxonomy `mvi_fa_file-status`. This lets you do conditional filtering on the frontend with many themes.
- Download files are stored in /{root}/file_downloads/secure/ so that you can set custom folder permissions and prevent direct access.
- Download files are not added to Wordpress media. That way they can't be accidentally deleted
- Once a week, the plugin will export a CSV will all submissions. CSV files are generated above the root in /csvoutput/.
- If the recaptcha key and secret are configured, then the download form will use them.
- If Mailchimp API settings are configured, then a subscribe option will be added to the form.
  - When users subscribe, they are automatically tagged with 'Downloaded PDF,' 'the_title_of_the_post_they_downloaded_from,' and 'their_professional_role.'
  - If a user is already subscribed to the Mailchimp list, then their tags will be updated every time they submit the form.
  - A user can only have one professional role tag in Mailchimp at a time.
  - The user will see a response on the frontend if their subscription succeeded.

## Use
1. Download and activate the plugin
2. Go to File Downloads -> Settings in the Wordpress admin.
3. Configure the settings. Most important are the 'From Address', 'From Name', 'Email CSV exports' and 'Select Post Types' fields. The rest are optional.
4. Navigate to a post to attatch a download file (must be a post type selected in Settings). Scroll to the bottom and upload the desired file.
  1. Optional: add custom title text for all of the forms on that post.
  2. Optional: add a custom short title for all forms on that post.
5. Add the shortcode `[mvi_fa_frontend_form]` to your post. Could be done in a single post template or manually. This will render a download form if the post has a file attached.
  1. If you would like to render the title only use `[mvi_fa_frontend_form form="false"]`. If you would like to render the short title use `[mvi_fa_frontend_form form="false" use_short_title="true"]`.
  2. If you would like to output a custom title for one instance of the form, you can override form title in the shortcode: `[mvi_fa_frontend_form title="My custom title"]`.

###Example setup
1. Use a template or theme hook to insert `[mvi_fa_frontend_form]` on every single-post, page, CPT. Can be put anywhere on the page, including in a modal.
2. Create a CTA block/template/etc. Use `[mvi_fa_frontend_form form="false"]` as the tile for the CTA and use `[mvi_fa_frontend_form form="false" use_short_title="true"]` as the button text. The button can link to an anchor or modal.
3. Show your CTA on all posts that have the term `has_file`.
