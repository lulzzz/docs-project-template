This starter Markdown documentation project supports Health Catalyst teams that want to begin managing their documentation as Markdown files. It uses the open-source tool [DocFx](https://dotnet.github.io/docfx) to generate a website and PDFs. The output aligns with Health Catalyst's [Fabric.Cashmere](https://github.com/HealthCatalyst/Fabric.Cashmere) style guide.

For an example of the web output, Markdown cheatsheets, and details on things like icons, callouts, styles, etc., see [docs.healthcatalyst.com](https://docs.healthcatalyst.com).

The following instructions apply to Windows, but DocFx and this project are also supported on Linux and macOS.

## Install DocFx
- Create a folder in `C:\Program Files` called `docfx`.
- Download `docfx.zip` from the [DocFx releases page](https://github.com/dotnet/docfx/releases/latest).
- Extract the contents to `C:\Program Files\docfx`.

### If you want to generate PDFs
- Download the [wkhtmltopdf installer](https://wkhtmltopdf.org/downloads.html).
- Run the installer. Use the default `C:\Program Files\wkhtmltopdf` as the destination folder.

### Add DocFx and wkhtmltopdf to `PATH` (in Windows)
- **Control Panel** > Change **View by** to **Small icons** > **System** > **Advanced system settings** > **Environment Variables...**
- Under the **System variables** section, scroll to **Path**. Select it and click **Edit...**.
- Depending on what version of Windows you're using, you see either a long string of values separated by semicolons or values in a vertical list.
  - If you see a long string of values, append to the end: `;C:\Program Files\DocFX;C:\Program Files\wkhtmltopdf\bin`
  - If you see values in a vertical list: Click **New** and paste `C:\Program Files\DocFX`. Click **New** again and paste `C:\Program Files\wkhtmltopdf\bin`.

## Build this project as a website
- On [this project's Github page](https://github.com/HealthCatalyst/docs-project-template), click **Clone or download** > **Download ZIP**. Extract it to wherever is convenient. This is the directory you'll work in. Don't extract it to `C:\Program Files\docfx`. 
- Open Windows Command Prompt. Enter `cd` followed by the filepath to your directory. For example, `cd C:\docs-project-template-master`.
- Run `docfx build docfx.json --serve`.
- In your browser, go to [http://localhost:8080](http://localhost:8080).

### For an easy internal website
- Find the website files in the newly generated subdirectory `\_site` in your directory.
- Push the site to an Azure web app (see [Local Git Deployment to Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-deploy-local-git)).
- Set up authentication to be internal visitors only with Azure Active Directory (see [Configure your App Service app to use Azure Active Directory login](https://docs.microsoft.com/en-us/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication)).
- Request a healthcatalyst.com or healthcatalyst.net domain name from IT (for example, mydocs.healthcatalyst.com).

## Generate PDFs
- Run `docfx pdf` in Windows Command Prompt.
- The PDFs are in the newly generated subdirectory `_site_pdf\docs-project-template-master_pdf` in your directory.

## How to change the order of articles
- The `toc.yml` file in `articles` determines the tabs in the menu bar of the site.
- The `toc.yml` files in `articles/<guide>` determine the order of articles in the site.
- The `toc.yml` files in `pdf/<guide>` determine the order of articles in the PDF.

## If you want to use Font Awesome icons
[Font Awesome](https://fontawesome.com) is the icon font used in Catalyst apps.
- Install [Node.js](https://nodejs.org).
- Close and reopen Windows Command Prompt. Enter `cd` followed by the filepath to your directory. For example, `cd C:\docs-project-template-master`.
- Enter `npm install --save`.
- How to include an icon in a Markdown file: `<i class="fa fa-plus"></i>` (in this example, replace `fa-plus` with `fa-name-of-icon` found in this [cheatsheet](https://fontawesome.com/cheatsheet))

## Other sample DocFx documentation projects
### Starter project
- Create a directory for your project.
- Inside that directory, open Windows Command Prompt.
- Run `docfx init -q`.

### Sample project
- Clone [DocFx seed](https://github.com/docascode/docfx-seed).

### Walkthroughs
See [DocFx walkthroughs](https://dotnet.github.io/docfx/tutorial/walkthrough/walkthrough_overview.html).

## Complication with the PDF title pages
The table of contents is always page 1. If you want a title page, create the title page as page 2 and use this script to flip the them.
- Install [PDFtk Free](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit). On the final window of the installer, choose not to open the program.
- Install [Windows Git](https://git-scm.com/download). This installs the Git Bash command-line tool, which allows you to run the Bash script that flips the pages.
- Open Git Bash and `cd` to your directory of generated PDFs. In Bash, use forward slashes in the directory path instead of back slashes (for example, `cd C:/docs-project-template-master/_site_pdf/docs-project-template-master_pdf`).
- Copy the following block of code. Right-click in Git Bash and select paste.
```
for filename in *.pdf; do
pdftk "$filename" cat 2-1 3-end output "$(date +%F)_$filename"
done
```
- Each new PDF's filename is prepended with today's date.
- Before you run `docfx pdf` again, delete the existing PDF(s).

## Credits
The site template is based on [https://github.com/MathewSachin/docfx-tmpl](https://github.com/MathewSachin/docfx-tmpl).