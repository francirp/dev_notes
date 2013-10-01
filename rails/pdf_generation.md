PDF Generation with Wicked PDF
==============================

**Links**
* WickedPDF: https://github.com/mileszs/wicked_pdf
* wkhtmltopdf: https://code.google.com/p/wkhtmltopdf/

Step #1: Install wkhtmltopdf
----------------------------

wkhtmltopdf is a shell utility to convert html to pdf using the webkit rendering engine, and qt.

```
brew install wkhtmltopdf
```

To test that it's working, generate a PDF of google's homepage:

```
wkhtmltopdf http://www.google.com google.pdf
```


Step #2: Wicked PDF in Rails
----------------------------

**Gemfile**
```ruby
gem 'wicked_pdf'
```

Then create the following initializer file to let Rails know where wkhtmltopdf exists on your development computer:

**config/initializers/wicked_pdf.rb**
```ruby
require 'wicked_pdf'
require 'wicked_pdf_tempfile'

if Rails.env == "development"
  WickedPdf.config = { :exe_path => '/usr/local/bin/wkhtmltopdf' }
end
```

```ruby
class ThingsController < ApplicationController
  def show
    respond_to do |format|
      format.html
      format.pdf do
        render :pdf => "file_name"
      end
    end
  end
end
```

**Wicked PDF Helpers**

In order to include your css and javascript in your PDF's, you will need to use the Wicked PDF helper methods.

Create the following file: views/layouts/application.pdf.erb

Then add the following, where pdf.css is the appropriate stylesheet for your PDF.

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
   "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <%= wicked_pdf_stylesheet_link_tag "pdf.css" %>
    <%= wicked_pdf_javascript_include_tag "number_pages" %>
  </head>
  <body onload='number_pages'>
    <div id="header">
      <%= wicked_pdf_image_tag 'mysite.jpg' %>
    </div>
    <div id="content">
      <%= yield %>
    </div>
  </body>
</html>
```

**Advanced Usage with all Available Options**

```ruby
class ThingsController < ApplicationController
  def show
    respond_to do |format|
      format.html
      format.pdf do
        render :pdf                            => 'file_name',
               :disposition                    => 'attachment',                 # default 'inline'
               :template                       => 'things/show.pdf.erb',
               :file                           => "#{Rails.root}/files/foo.erb"
               :layout                         => 'pdf.html',                   # use 'pdf.html' for a pdf.html.erb file
               :wkhtmltopdf                    => '/usr/local/bin/wkhtmltopdf', # path to binary
               :show_as_html                   => params[:debug].present?,      # allow debuging based on url param
               :orientation                    => 'Landscape',                  # default Portrait
               :page_size                      => 'A4, Letter, ...',            # default A4
               :save_to_file                   => Rails.root.join('pdfs', "#{filename}.pdf"),
               :save_only                      => false,                        # depends on :save_to_file being set first
               :proxy                          => 'TEXT',
               :basic_auth                     => false                         # when true username & password are automatically sent from session
               :username                       => 'TEXT',
               :password                       => 'TEXT',
               :cover                          => 'URL',
               :dpi                            => 'dpi',
               :encoding                       => 'TEXT',
               :user_style_sheet               => 'URL',
               :cookie                         => ['_session_id SESSION_ID'], # could be an array or a single string in a 'name value' format
               :post                           => ['query QUERY_PARAM'],      # could be an array or a single string in a 'name value' format
               :redirect_delay                 => NUMBER,
               :javascript_delay               => NUMBER,
               :zoom                           => FLOAT,
               :page_offset                    => NUMBER,
               :book                           => true,
               :default_header                 => true,
               :disable_javascript             => false,
               :grayscale                      => true,
               :lowquality                     => true,
               :enable_plugins                 => true,
               :disable_internal_links         => true,
               :disable_external_links         => true,
               :print_media_type               => true,
               :disable_smart_shrinking        => true,
               :use_xserver                    => true,
               :no_background                  => true,
               :extra                          => ''                        # directly inserted into the command to wkhtmltopdf
               :margin => {:top                => SIZE,                     # default 10 (mm)
                           :bottom             => SIZE,
                           :left               => SIZE,
                           :right              => SIZE},
               :header => {:html => { :template => 'users/header.pdf.erb',  # use :template OR :url
                                      :layout   => 'pdf_plain.html',        # optional, use 'pdf_plain.html' for a pdf_plain.html.erb file, defaults to main layout
                                      :url      => 'www.example.com',
                                      :locals   => { :foo => @bar }},
                           :center             => 'TEXT',
                           :font_name          => 'NAME',
                           :font_size          => SIZE,
                           :left               => 'TEXT',
                           :right              => 'TEXT',
                           :spacing            => REAL,
                           :line               => true,
                           :content            => 'HTML CONTENT ALREADY RENDERED'}, # optionally you can pass plain html already rendered (useful if using pdf_from_string)
               :footer => {:html => { :template => 'shared/footer.pdf.erb', # use :template OR :url
                                      :layout   => 'pdf_plain.html',        # optional, use 'pdf_plain.html' for a pdf_plain.html.erb file, defaults to main layout
                                      :url      => 'www.example.com',
                                      :locals   => { :foo => @bar }},
                           :center             => 'TEXT',
                           :font_name          => 'NAME',
                           :font_size          => SIZE,
                           :left               => 'TEXT',
                           :right              => 'TEXT',
                           :spacing            => REAL,
                           :line               => true,
                           :content            => 'HTML CONTENT ALREADY RENDERED'}, # optionally you can pass plain html already rendered (useful if using pdf_from_string)
               :toc    => {:font_name          => "NAME",
                           :depth              => LEVEL,
                           :header_text        => "TEXT",
                           :header_fs          => SIZE,
                           :l1_font_size       => SIZE,
                           :l2_font_size       => SIZE,
                           :l3_font_size       => SIZE,
                           :l4_font_size       => SIZE,
                           :l5_font_size       => SIZE,
                           :l6_font_size       => SIZE,
                           :l7_font_size       => SIZE,
                           :l1_indentation     => NUM,
                           :l2_indentation     => NUM,
                           :l3_indentation     => NUM,
                           :l4_indentation     => NUM,
                           :l5_indentation     => NUM,
                           :l6_indentation     => NUM,
                           :l7_indentation     => NUM,
                           :no_dots            => true,
                           :disable_links      => true,
                           :disable_back_links => true},
               :outline => {:outline           => true,
                            :outline_depth     => LEVEL}
      end
    end
  end
end
```
