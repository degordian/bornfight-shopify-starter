# Bornfight starter theme for Shopify
This is a start kit for Shopify projects. Kit is runned by Shopify Slate and based on Shopify's starter theme. SCSS structure is Bornfight's project starter kit standard.

[Slate v1 documentation](https://github.com/Shopify/slate)
[Shopify starter theme](https://github.com/Shopify/starter-theme)

## Creating new Shopify project with bornfight-shopify-starter
1) Create development store 
2) Open terminal and Projects/Work folder
3) Run `yarn create slate-theme [THEME_NAME] degordian/bornfight-shopify-starter`
For example `yarn create slate-theme my-new-theme degordian/bornfight-shopify-starter`
Project should now be ready to go, with installed npm
4) Open project in PHP storm and run `yarn zip`, that will build and zip the files as a Shopify theme.
5) Upload that theme to developemnt store from first step (Online store > Themes > Upload theme), publish it.
6) Get theme ID - when theme is published it will be on the first place in theme list, click Actions > Edit code. This will open Shopify code editor, theme ID is last set of numbers in browser url after /themes/. For example `https://your-shop-name.myshopify.com/admin/themes/20842139465` . So theme ID here is `20842139465`. You will need this when you connect online theme with local theme.
7) Create a new private app by navigating to your store’s private apps page (Apps > Manage private apps), giving the private app a name and setting the Theme templates and theme assets to “Read and write”. This will generate a password needed for connecting online theme with local theme.
8) Go back to PHP storm and find and edit `.env` file, this is config file for connecting local theme with development store
For example:
```
# The myshopify.com URL to your Shopify store
SLATE_STORE={store-name}.myshopify.com

# The API password generated from a Private App
SLATE_PASSWORD=ccf7fb19ed4dc6993ac6355c0c489c7c7

# The ID of the theme you wish to upload files to
SLATE_THEME_ID=32112656003

# A list of file patterns to ignore, with each list item separated by ':'
SLATE_IGNORE_FILES=config/settings_data.json
```
Where 
- SLATE_STORE is url to development store without htpps
- SLATE_PASSWORD is password from step 7
- SLATE_THEME_ID is theme ID from step 6
- SLATE_IGNORE_FILES are files that gets ignored when deploying. Its recommended to ignore `settings_data.json` as in example, so you dont overwrite `settings_data.json` from live theme because it contains all of your content.

## Working with Shopify slate
Slate will open local server for development and manage your assets with webpack. It will also deploy your theme as production ready theme. Here are Slate commands. 

- `yarn build` - Builds a production-ready version of the theme by compiling the files into the dist folder.
- `yarn deploy` - Uploads the dist folder to the Shopify store.
- `yarn format` - Formats the theme code according to the rules declared in the .eslintrc and .stylelintrc files. By default, it uses ESLint Fix to format JS files, Stylelint Fix to format CSS files and Prettier to format JSON files.
- `yarn lint` - Runs linting
- `yarn start` - Compiles your local theme files into a dist directory, uploads these files to your remote Shopify store and finally boots up a local Express server that will serve most of your CSS and JavaScript.
- `yarn zip`  - Compiles the contents of the dist directory and creates a ZIP file in the root of the project.

So to start developing you must rund `yarn start`, and for deployment `yarn deploy`

## Writing Liquid
[Liquid documentation](https://help.shopify.com/en/themes/liquid)
When writing liquid code, generated HTML is bloated with whitespace.
For example, calling a snippet `{% include 'snippet' %}` produces:

```
<select>

  <option value="GBP" selected="selected">GBP</option>


    <option value="INR">INR</option>



    <option value="CAD">CAD</option>


  </select>
  ```
  
This makes generated HTML very big in number of lines. Liquid has a whitespace control to strip whitespace. You can include a hyphen in your tag syntax `{{-`, `-}}`, `{%-`, and `-%}` to strip whitespace from the left or right side of a rendered tag.
  
So then if you call the same snipet with hyphen's - `{%- include 'snippet' -%}` This will generate much cleaner code:
  
  ```
  <select><option value="GBP" selected="selected">GBP</option><option value="INR">INR</option><option value="CAD">CAD</option></select>
  ```
  
Dont use Shopify whitespace control all the time. For example if you have some logical tags for classes and use whitespace control:

```
<body id="{{ page_title | handle }}" class="template-{{- template.name | handle -}} {%- if template.name == 'homepage' -%} template--homepage{%- endif -%}">
```

This will not produce whitespace where it needs to be and this will not work: 

```
<body id="bornfight-starkit" class="template-indextemplate--homepage">
```

Also as seen in upper example on `id="{{ page_title | handle }}"` there is no need to use whitespace control in the variables in HTML tags, Liquid does not produce it there.

## Accessing images from asset folder

```
<img src="{{ 'logo.png' | asset_img_url: '300x' }}" alt="">
```

[Image url and url filters docs](https://help.shopify.com/en/themes/liquid/filters/url-filters#asset_img_url)

## Working with SVG icons
SVG icons are handled as snippets. When adding new just open a file prefixed with `icon-` inside `snippets` folder and copy SVG code. Then  you can call any SVG icon in any file you need with:

```
{%- include 'icon-instagram' -%}
```

## License
MIT © [Bornfight](https://www.bornfight.com/)
