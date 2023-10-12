---
layout: post
title: "Automation Me"
subtitle: "Creating a Workflow for Blogging"
date: 2023-10-12
readtime: true
tags: [PowerShell, Plaster, Jekyll, REST]
cover-img: ["/assets/img/automation-me/automation-me.jpg" : "by [https://pixabay.com/users/Hans-2] via Pixabay"]
thumbnail-img: /assets/img/automation-me/automation_sm.jpg
share-img: /assets/img/automation-me/automation_sm.jpg
---

<!--more-->

# Intro
On September 20th, 2023, I spoke about my blogging history and workflow using PowerShell at the [**Research Triangle Park PowerShell User Group**](https://rtpsug.com/){:target="_blank"}. I used the new workflow scripts to create this blog. There's a link to my GitHub repo in the [**Learning More**](http://localhost:4000/2023-10-12-automation-me/?utm_source=blog&utm_medium=blog&utm_content=recent#learning-more) section that contains the scripts and files used.

You can find the recording of the presentation [**here**](https://youtu.be/uiYuOab2B3w?si=1SNbH7WN1bF1Bnp0).

# Contents

* TOC
{:toc}

# Beginning my Blogging Journey
I started the [**WagtheReal.com**](http://web.archive.org/web/20201209123724/https://wagthereal.com/) blog in 2009 while working as a contractor at the CDC in Atlanta.   

<img src="/assets/img/automation-me/batman_itscitrix.jpg" width="400" height="400">{: .mx-auto.d-block :}  

It was an opportunity to share with the larger Citrix community and highlight the PowerShell I was using to manage our environment. I decided on Wordpress.com since it was free and easy to use.

| Early Format | Later Format |
| :----: | :----: |
| <img src="/assets/img/automation-me/earlyWTR.png"> | <img src="/assets/img/automation-me/whereAreApps.png"> |

# Learning Git
After a few years and other jobs, I had written a sizable library of PowerShell functions, scripts, and modules. Much like rolling over a 401k to a new employer, I would bring all my previous code to my next job. I do not exactly recall when I started forcing myself to learn Git, but I had been reading about it.  I became interested in version control,  branching, and other aspects of Git that I thought would help me become a better PowerShell developer.  

Consequently, the Research Triangle PowerShell User Group had speakers presenting about Git and had a great interactive demonstration about making and contributing to a Git repo on [**GitHub**](https://github.com).
<iframe width="560" height="315" src="https://www.youtube.com/embed/IZHbX8PJRWM?si=sQjlVC3sEjhJbuZZ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>{: .mx-auto.d-block :}

After some trial and error, I created my first repo on GitHub in February 9<sup>th</sup> 2017. The process of using Git daily, writing in Markdown, and other aspects led me to start looking at GitHubPages as a potential place to blog.

| :----: | :----: |
| <img src="/assets/img/automation-me/github-profile.png"> | <img src="/assets/img/automation-me/run-delprof2.png"> |

Moving to GitHub Pages would give me more control in the design and look of my site and enforce the skills I was trying to learn.

# GitHub Pages
I liked the idea of static websites using Jekyll  and since GitHub pages used Jekyll, I thought it would be straight-forward to get [**Ruby**](https://www.ruby-lang.org/en/) and [**Jekyll**](https://jekyllrb.com/) running locally using [**Windows Subsystem for Linux**](https://learn.microsoft.com/en-us/windows/wsl/about).  

As is the case for a lot of blogs, the information quickly stales. I had to use several tutorials and Stack Exchange to get an environment working, but I finally got it down and started creating blog posts. I created **PowerEUCShell** based on the [**Beautiful Jekyll**](https://beautifuljekyll.com/) theme. 

<img src="/assets/img/automation-me/PowerEUCShell.png" width="600">{: .mx-auto.d-block :}  

I perferred writing locally as I could write a draft post, edit it, setup pictures, and do all my development and review locally before I pushed it to my GitHub repo. Then the built-in GitHub Actions would compile and update my blog site.

After running WSL and writing blogs locally for a few months, I had an OS issue and my WSL got corrupted. I did not relish the thought of recreating the Frankenstein-like Jekyll install (not to mention writing down all the steps to recreate the environment) so I thought about looking at Docker to make the entire process simpler.

# Docker

Using [**Docker**](https://www.docker.com/) turned out to be a lot easier to setup and recreate if any issues occurred. I cannot recommend enough the videos by **Bill Raymond**. He does a great job walking you step by step on setting up your system to develop Jekyll locally.

<iframe width="560" height="315" src="https://www.youtube.com/embed/zijOXpZzdvs?si=mbnjGPgUYBlsBYCJ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>{: .mx-auto.d-block :}

Since I had to start over, I went ahead and upgraded the latest version of the Beautiful Jekyll theme and (finally) decided to self-brand my site. I changed my site's URL from **PowerEUCShell.githubpages.io** to  [**alainassaf.com**](https://alainassaf.com).  I also went through the lengthy process of converting almost all my previous WordPress posts to Jekyll. This was mostly manual and took a couple of months to move 170+ posts.

With all this done, I wanted to make it easy to set up a new blog post so I started digging into Plaster, a function to grab an image, and a wrapper script to run everything.

# Plaster

| What It Does | How Do You Do It |
| :--- | :--- |
| Plaster is a template-based file and project generator written in PowerShell. | You create a Plaster Manifest file in XML format |
| Its purpose is to streamline the creation of PowerShell module projects, Pester tests, DSC configurations, and more. | You can set metadata, parameters, and content. |
| File generation is performed using crafted templates which allow the user to fill in details and choose from options to get their desired output. | You can prompt the user for input like a file name or whether to include folders. |

{: .box-note}
**FYI:** Microsoft transferred ownership of Plaster to PowerShell.org in 2020.

Covering all the ins and outs of using Plaster is outside the scope of this blog. Here are a few great blogs that I used to learn about Plaster:
* [**PowerShell: Adventures in Plaster (powershellexplained.com)**](https://powershellexplained.com/2017-05-12-Powershell-Plaster-adventures-in/)
* [**PowerShell: GetPlastered a Plaster template to create a Plaster template (powershellexplained.com)**](https://powershellexplained.com/2017-05-14-Powershell-Plaster-GetPlastered-template/)
* [**Working With Plaster — OverPoweredShell.com — By David Christian**](https://overpoweredshell.com/Working-with-Plaster/)

In my setup, I have 2 files that drive the creation process. One is my Plaster manifest file which sets up files and folders. The other is a template file that is used to create the blank markdown file used to write blog posts.

## PlasterManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<plasterManifest schemaVersion="1.0"
  xmlns="http://www.microsoft.com/schemas/PowerShell/Plaster/v1">
  <metadata>
    <name>BlogPost</name>
    <id>a22bb4ac-783a-40c1-beef-9ec4c89e378a</id>
    <version>0.0.1</version>
    <title>Blog Post Template</title>
    <description>New draft blog post for alainassaf.com</description>
    <author>Alain Assaf</author>
    <tags>BlogPost</tags>
  </metadata>
  <parameters>
    <parameter name="Title" type="text" prompt="Title of blog post" />
    <parameter name="subtitle" type='choice' store='text' prompt="Include blog subtitle?" default='1'>
      <choice label='&amp;Yes'
              help='Adds a blog subtitle'
              value='Yes' />
      <choice label='&amp;No'
              help='Does not add a blog subtitle'
              value='No' />
    </parameter>
    <parameter name="SubTitleText" type="text" prompt="Enter blog post subtitle" condition='$PLASTER_PARAM_SubTitle -eq "Yes"' />
    <parameter name="BlogFileName" type="text" prompt="Blog File Name" default="$(${PLASTER_PARAM_Title}.replace(' ','-').tolower())" />
    <parameter name="Tags" type="text" prompt="Tags" default=""/>
    <parameter name="Date" type="text" prompt="Publish Date" default="$(get-date -Format yyyy-MM-dd)" />
  </parameters>
  <content>
    <message>Creating blank post in /drafts folder</message>
    <file source='' destination='_drafts'/>
    <message condition='$PLASTER_PARAM_SubTitle -eq "Yes"'>Blog has subtitle</message>
    <message condition='$PLASTER_PARAM_SubTitle -eq "No"'>Blog does not have a subtitle</message>
    <templateFile source="BlogPost.asp1"
                  destination='_drafts\${PLASTER_PARAM_Date}-${PLASTER_PARAM_BlogFileName}.md'/>
    <message>Creating empty folder for images - /assets/img/${PLASTER_PARAM_BlogFileName}</message>
    <file source ='' destination='$("../alainassaf.github.io/assets/img/${PLASTER_PARAM_BlogFileName}")'/>
  </content>
</plasterManifest>
```

We can break the manifest file into 3 sections

### metadata
The *metadata* section lists the properties of the manifest. These were used when creating a new PlasterManifest (i.e. `New-PlasterManifest`).
```xml
<metadata>
    <name>BlogPost</name>
    <id>a22bb4ac-783a-40c1-beef-9ec4c89e378a</id>
    <version>0.0.1</version>
    <title>Blog Post Template</title>
    <description>New draft blog post for alainassaf.com</description>
    <author>Alain Assaf</author>
    <tags>BlogPost</tags>
  </metadata>
```

### parameters
The *parameters* section prompts the user for input (if needed) and sets variables used in the *content* section.
```xml
<parameters>
    <parameter name="Title" type="text" prompt="Title of blog post" />
    <parameter name="subtitle" type='choice' store='text' prompt="Include blog subtitle?" default='1'>
      <choice label='&amp;Yes'
              help='Adds a blog subtitle'
              value='Yes' />
      <choice label='&amp;No'
              help='Does not add a blog subtitle'
              value='No' />
    </parameter>
    <parameter name="SubTitleText" type="text" prompt="Enter blog post subtitle" condition='$PLASTER_PARAM_SubTitle -eq "Yes"' />
    <parameter name="BlogFileName" type="text" prompt="Blog File Name" default="$(${PLASTER_PARAM_Title}.replace(' ','-').tolower())" />
    <parameter name="Tags" type="text" prompt="Tags" default=""/>
    <parameter name="Date" type="text" prompt="Publish Date" default="$(get-date -Format yyyy-MM-dd)" />
  </parameters>
```

### content
The *content* section creates the files and folders based on the *metadata* and *parameters* section.
```xml
<content>
    <message>Creating blank post in /drafts folder</message>
    <file source='' destination='_drafts'/>
    <message condition='$PLASTER_PARAM_SubTitle -eq "Yes"'>Blog has subtitle</message>
    <message condition='$PLASTER_PARAM_SubTitle -eq "No"'>Blog does not have a subtitle</message>
    <templateFile source="BlogPost.asp1"
                  destination='_drafts\${PLASTER_PARAM_Date}-${PLASTER_PARAM_BlogFileName}.md'/>
    <message>Creating empty folder for images - /assets/img/${PLASTER_PARAM_BlogFileName}</message>
    <file source ='' destination='$("../alainassaf.github.io/assets/img/${PLASTER_PARAM_BlogFileName}")'/>
  </content>
```
## BlogPost.asp1
The *BlogPost.asp1* is used by Plaster to create a template for my new blog post in the *MarkDown* language.
```yaml
<%
    if ($PLASTER_PARAM_SubTitle -eq 'Yes') {
        @"
---
layout: post
title: "<%= $PLASTER_PARAM_Title %>"
subtitle: "<%= $PLASTER_PARAM_SubTitleText %>"
date: <%= $PLASTER_PARAM_Date %>
readtime: true
tags: [<%= $PLASTER_PARAM_Tags %>]
cover-img: ["/assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg" : "by [] via Pixabay"]
thumbnail-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
share-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
---

<!--more-->

# Contents

* TOC
{:toc}

# Scenario

# Body

# Conclusion

# Learning More

### Value for Value
If you received any value from reading this post, please [**buy me a coffee**](https://www.buymeacoffee.com/j72aXgIYJh){:target="_blank"} to show your support.
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="j72aXgIYJh" data-color="#16609f" data-emoji="☕"  data-font="Arial" data-text="Buy me a coffee" data-outline-color="#ffffff" data-font-color="#ffffff" data-coffee-color="#FFDD00" ></script>

*Thanks for Reading,*  
*Alain Assaf*
"@
    } else {
        @"
---
layout: post
title: "<%= $PLASTER_PARAM_Title %>"
date: <%= $PLASTER_PARAM_Date %>
readtime: true
tags: [<%= $PLASTER_PARAM_Tags %>]
cover-img: ["/assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg" : "Pixabay"]
thumbnail-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
share-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
---

<!--more-->

# Contents

* TOC
{:toc}

# Scenario

# Body

# Conclusion

# Learning More

### Value for Value
If you received any value from reading this post, please [**buy me a coffee**](https://www.buymeacoffee.com/j72aXgIYJh){:target="_blank"} to show your support.
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="j72aXgIYJh" data-color="#16609f" data-emoji="☕"  data-font="Arial" data-text="Buy me a coffee" data-outline-color="#ffffff" data-font-color="#ffffff" data-coffee-color="#FFDD00" ></script>

*Thanks for Reading,*  
*Alain Assaf*
"@
    }

%>
```

In my *PlasterManifiest.xml*, I prompt to include a subtitle with the blog with this section:  
```XML
 <parameter name="subtitle" type='choice' store='text' prompt="Include blog subtitle?" default='1'>
      <choice label='&amp;Yes'
              help='Adds a blog subtitle'
              value='Yes' />
      <choice label='&amp;No'
              help='Does not add a blog subtitle'
              value='No' />
    </parameter>
```

This is why most of the template file is duplicated. The top section creates the blog file with a subtitle, while the bottom section omits it. This is highlighted in the YAML header of each section.

#### Subtitle
```yml
---
layout: post
title: "<%= $PLASTER_PARAM_Title %>"
subtitle: "<%= $PLASTER_PARAM_SubTitleText %>"
date: <%= $PLASTER_PARAM_Date %>
readtime: true
tags: [<%= $PLASTER_PARAM_Tags %>]
cover-img: ["/assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg" : "by [] via Pixabay"]
thumbnail-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
share-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
---
```

#### No subtitle
```yml
---
layout: post
title: "<%= $PLASTER_PARAM_Title %>"
date: <%= $PLASTER_PARAM_Date %>
readtime: true
tags: [<%= $PLASTER_PARAM_Tags %>]
cover-img: ["/assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg" : "Pixabay"]
thumbnail-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
share-img: /assets/img/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>/<%= ($PLASTER_PARAM_title).replace(' ','-').tolower() %>.jpg
---
```
# Get-Pixabayimage function
Back in 2020, I wanted an easier way to get a (royalty-free) image for my blogs. For years, I had crafted one based on an image meme and sometimes this took me longer to craft than the blog itself. After doing some research, I discovered Pixabay had lots of royalty-free photos and an easy-to-understand API. Once again, a presentation from the Research Triangle PowerShell User Group inspired me to try writing my own REST API function to grab a file from Pixabay.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZbpbissNlCs?si=gPjjPtki7toqfB0I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>{: .mx-auto.d-block :}

Armed with this info from the RTPUG, I wrote a post about the Pixabay API and my function in April of 2020. For a deep dive into Pixabay's REST API and how I wrote a function to use it you can read more here: [**PowerShell: Invoke-Restmethod**](https://alainassaf.com/2020-04-22-Powershell-Invoke-RestMethod/). 

I revisited this function and made some minor changes to make fit better into my workflow.

```posh
function get-pixabayImage {
    <#
    .SYNOPSIS
    Uses Pixabay REST API to download an image from the Pixabay.com site
    .DESCRIPTION
    Uses Pixabay REST API to download an image from the Pixabay.com site. 
    The script also saves the image's user for attribution.
    .PARAMETER Query
    Mandatory string parameter to use for the Pixabay image query
    .PARAMETER Category
    Mandatory string parameter used for the Pixabay image category.
    Must be one of the following values: backgrounds
                                        fashion
                                        nature
                                        science
                                        education
                                        feelings
                                        health
                                        people
                                        religion
                                        places
                                        animals
                                        industry
                                        computer
                                        food
                                        sports
                                        transportation
                                        travel
                                        buildings
                                        business
                                        music
    .PARAMETER Color
    Optional string parameter used for the Pixabay image color.
    Must be one of the following values: grayscale
                                        transparent
                                        red
                                        orange
                                        yellow
                                        green
                                        turquoise
                                        blue
                                        lilac
                                        pink
                                        white
                                        gray
                                        black
                                        brown
    .PARAMETER apikey
    Mandatory string parameter used to send your user apikey to Pixbay.
    See https://pixabay.com/api/docs/ on how to create your own APIKey.
    .EXAMPLE
    get-pixabayImage -query computer -category computer -color green -apikey SOMEAPIKEYHERE -Verbose
    Query Pixabay with 'computer' and use the category computer and color green 
    with additional feedback.
    .INPUTS
    None
    .OUTPUTS
    Image file (jpeg) from Pixabay and text file with image user for attribution.
    .NOTES
    NAME: get-pixabayImage.ps1
    VERSION: 1.0.4
    CHANGE LOG - Version - When - What - Who
    1.0.0 - 09/12/2023 - Initial script - Alain Assaf
    1.0.1 - 09/15/2023 - Add more params to API query - Alain Assaf
    1.0.2 - 09/15/2023 - Add error check if less than 50 results - Alain Assaf
    1.0.3 - 10/03/2023 - Made apikey mandatory - Alain Assaf
    1.0.4 - 10/03/2023 - Updated parameter arguments syntax - Alain Assaf
    AUTHOR: Alain Assaf
    LASTEDIT: October 03, 2023
    .LINK
    https://github.com/alainassaf/new_alainassaf_post
    https://pixabay.com/api/docs/
    http: //www.linkedin.com/in/alainassaf/
    #>
    [CmdletBinding()]
    param (
        [parameter(Position = 0, Mandatory)]    
        [ValidateNotNullOrEmpty()]
        [string]$query,

        [Parameter(Mandatory)]
        [ValidateSet("backgrounds", "fashion", "nature", "science", "education", "feelings", "health", "people", "religion", "places", "animals", "industry", "computer", "food", "sports", "transportation", "travel", "buildings", "business", "music")]
        $category,

        [Parameter()]
        [ValidateSet("grayscale", "transparent", "red", "orange", "yellow", "green", "turquoise", "blue", "lilac", "pink", "white", "gray", "black", "brown")]
        $color,

        #Pixabay apikey
        [parameter(Mandatory)]
        [string]$apikey
    )
    
    #region variables
    $datetime = Get-Date -Format "MM-dd-yyyy_HH-mm"
    $ScriptRunner = (Get-ChildItem env:username).value
    $compname = (Get-ChildItem env:COMPUTERNAME).value
    $scriptName = $MyInvocation.MyCommand.Name
    $scriptpath = $MyInvocation.MyCommand.Path
    $error.clear()
    #endregion

    $picFilename = (Get-Date).tostring("yyyy-MM-dd-hh-mm-ss")
    $picFilename = $picFilename + ".jpg"

    #Reform query to URL encoded search term
    $URLSearch = $query.Replace(" ", "+")

    if ($null -ne $color) {
        $Body = @{
            key         = $apikey
            q           = $URLSearch
            category    = $category
            lang        = "en"
            image_type  = "photo"
            orientation = "horizontal"
            safesearch  = $true
            per_page    = 50
            order       = "popular"
        }
    } else {
        $Body = @{
            key         = $apikey
            q           = $URLSearch
            category    = $category
            lang        = "en"
            image_type  = "photo"
            orientation = "horizontal"
            safesearch  = $true
            color       = $color
            per_page    = 50
            order       = "popular"
        }
    }

    $splat = @{
        URI    = "https://pixabay.com/api/"
        Method = "Get"
        Body   = $Body
    }

    try {
        $pixabay_query = Invoke-RestMethod @splat
    } catch {
        Write-Warning "Failed to query Pixabay"
        break
    }
     
    if ($pixabay_query.totalHits -eq 0) {
        Write-Warning "No query results"
        Break
    } else {
        # Limiting query to 50 at a time. Choose a random number to pick an image.
        if ($pixabay_query.totalHits -lt 50) {
            $randomHit = Get-Random -Minimum 1 -Maximum $pixabay_query.totalHits
        } else {
            $randomHit = Get-Random -Minimum 1 -Maximum 50
        }
        $img = $pixabay_query.hits[$randomHit]
        
        #Image Info
        Write-Verbose "Filepath: [$PSScriptRoot]"
        $tmpURL = $img.pageURL
        Write-Verbose "Pixabay url = [$tmpURL]"
        $tmpTags = $img.tags
        write-verbose "Tags: [$tmpTags]"
        $tmpuser = $img.user
        $tmpuserid = $img.user_id
        Write-Verbose "Pixabay User URL = [https://pixabay.com/users/$tmpuser-$tmpuserid]"
        
        #Get image and save user info for attribution
        Invoke-WebRequest -Uri $img.webformatUrl -OutFile .\$picFileName
        $tmpPixabayUser = $picFilename.split('.')[0] + "_pixabayUser.txt"
        "https://pixabay.com/users/$tmpuser-$tmpuserid" | Out-File -FilePath .\$tmpPixabayUser -Append
        & ".\$picFileName"
    }   
    #End Script info
    Write-Verbose "SCRIPT NAME: $scriptName"
    Write-Verbose "SCRIPT PATH: $scriptPath"
    Write-Verbose "SCRIPT RUNTIME: $datetime"
    Write-Verbose "SCRIPT USER: $ScriptRunner"
    Write-Verbose "SCRIPT SYSTEM: $compname.$domain"
}
```

In addition to getting the image, I also grab the URL of the artist who uploaded the image to Pixabay. This allows me to give attribution of the image in my blog posts.

# Bringing It All Together
With my Plaster template and the get image function I have the 2 main pieces I need to create a new blog post. So, I wrote a wrapper script that invokes plaster and grabs and image and sets up a blank blog post that I can then use.

```posh
<#
.SYNOPSIS
This script takes a Plaster splat object and invokes the PlasterManifest for a new blog post
to alainassaf.com. It also grabs an image from Pixabay, renames it, and saves it the apprpriate
folder as the main image of the blog post.
.DESCRIPTION
This script takes a Plaster splat object and invokes the PlasterManifest for a new blog post
to alainassaf.com. It also grabs an image from Pixabay, renames it, and saves it the apprpriate
folder as the main image of the blog post.
.PARAMETER PlasterSplat
A hashtable with the Plaster manifest parameters.
Example:
$SplatExample = @{
    Title = "This Is a Test Post"
    TemplatePath ="D:\Codevault\PoSH\blog\new_alainassaf_post"
    DestinationPath = "D:\Codevault\PoSH\blog\alainassaf.github.io"
    subtitle = "Yes"
    SubTitleText = "This is a test subtitle"
    Tags = "test,post"
    Date = get-date -Format yyyy-MM-dd
}
.PARAMETER PathToGetPixabayFunction
Mandatory string parameter that is the full path to the get-pixabayimage.ps1 function.
This function is used to query the Pixabay website (using REST api) for an image
.PARAMETER ImageQuery
Mandatory string parameter to use for the Pixabay image query
.PARAMETER ImageCategory
Mandatory string parameter used for the Pixabay image category.
Must be one of the following values:    backgrounds
                                        fashion
                                        nature
                                        science
                                        education
                                        feelings
                                        health
                                        people
                                        religion
                                        places
                                        animals
                                        industry
                                        computer
                                        food
                                        sports
                                        transportation
                                        travel
                                        buildings
                                        business
                                        music
.PARAMETER ImageColor
Optional string parameter used for the Pixabay image color.
Must be one of the following values:    grayscale
                                        transparent
                                        red
                                        orange
                                        yellow
                                        green
                                        turquoise
                                        blue
                                        lilac
                                        pink
                                        white
                                        gray
                                        black
                                        brown
.PARAMETER apikey
Mandatory string parameter used to send your user apikey to Pixbay.
.EXAMPLE
Create a hashtable with the Plaster manifest parameters.
$plaster = @{
    Title = "This Is a Test Post"
    TemplatePath ="D:\Codevault\PoSH\blog\new_alainassaf_post"
    DestinationPath = "D:\Codevault\PoSH\blog\alainassaf.github.io"
    subtitle = "Yes"
    SubTitleText = "This is a test subtitle"
    Tags = "test,post"
    Date = get-date -Format yyyy-MM-dd
}

.\create-blogpost.ps1 -PlasterSplat $plaster -PathToGetPixabayFunction $pathToFunc -ImageQuery "flowers" -ImageCategory "backgrounds" -ImageColor "red" -apikey SOMEPIXABAYAPIKEY -Verbose
.INPUTS
Hashtable with Plaster manifest variables
.OUTPUTS
None
.NOTES
NAME: create-blogpost.ps1
VERSION: 1.0.5
CHANGE LOG - Version - When - What - Who
0.0.1 - 08/12/2023 - Initial script - Alain Assaf
0.0.2 - 09/11/2023 - Updated param for get-pixabayimage function - Alain Assaf
1.0.0 - 09/15/2023 - Added more comments - Alain Assaf
1.0.1 - 09/15/2023 - Made ImageColor optional param - Alain Assaf
1.0.2 - 09/18/2023 - Fixed spelling - Alain Assaf
1.0.3 - 10/03/2023 - Updated parameter arguments syntax - Alain Assaf
1.0.4 - 10/03/2023 - Selecting only latest jpg file - Alain Assaf
1.0.5 - 10/12/2023 - Added APIkey param - Alain Assaf
AUTHOR: Alain Assaf
LASTEDIT: October 12, 2023
.LINK
https://github.com/alainassaf/new_alainassaf_post
https://pixabay.com/api/docs/
http: //www.linkedin.com/in/alainassaf/
#>
#Requires -Modules Plaster

[CmdletBinding()]
param (
    [Parameter(Mandatory)]
    [ValidateNotNullOrEmpty()]
    [System.Collections.Hashtable]$PlasterSplat,

    [Parameter(Mandatory)]
    [ValidateNotNullOrEmpty()]
    [string]$PathToGetPixabayFunction,

    [parameter(Mandatory)]
    [ValidateNotNullOrEmpty()]
    [string]$ImageQuery,

    [Parameter(Mandatory)]
    [ValidateSet("backgrounds", "fashion", "nature", "science", "education", "feelings", "health", "people", "religion", "places", "animals", "industry", "computer", "food", "sports", "transportation", "travel", "buildings", "business", "music")]
    $ImageCategory,

    [Parameter()]
    [ValidateSet("grayscale", "transparent", "red", "orange", "yellow", "green", "turquoise", "blue", "lilac", "pink", "white", "gray", "black", "brown")]
    $ImageColor,

    #Pixabay apikey
    [parameter(Mandatory)]
    [string]$apikey
)

#region variables
$datetime = Get-Date -Format "MM-dd-yyyy_HH-mm"
$ScriptRunner = (Get-ChildItem env:username).value
$compname = (Get-ChildItem env:COMPUTERNAME).value
$scriptName = $MyInvocation.MyCommand.Name
$currentDir = Split-Path $MyInvocation.MyCommand.Path
#endregion

# Check paths
if (Test-Path $PathToGetPixabayFunction) {
    . $PathToGetPixabayFunction
    if (Test-Path $PlasterSplat.TemplatePath) {
        if (Test-Path $PlasterSplat.DestinationPath) {
            Write-Verbose "PlasterSplat param: $PlasterSplat"
            # Create blog post template
            Invoke-Plaster @PlasterSplat
        } else {
            Write-Warning "[$PlasterSplat.DestinationPath] not found"
            exit 1
        }
    } else {
        Write-Warning "[$PlasterSplat.TemplatePath] not found"
        exit 1
    }
} else {
    Write-Warning "[$PathToGetPixabayFunction] not found"
    exit 1
}

#Get name for main blog image
$blogImageName = $plaster.Title.replace(' ', '-').tolower() + ".jpg"
Write-Verbose "Main blog image name: [$blogImageName]"

#Query Pixabay for blog image
if ($ImageColor) {
    get-pixabayImage -query $ImageQuery -category $ImageCategory -color $ImageColor -apikey $ApiKey
} else {
    get-pixabayImage -query $ImageQuery -category $ImageCategory -apikey $ApiKey
}

Start-Sleep -Seconds 5

#Only use the latest image for the blog post.
$BlogImage = Get-ChildItem -Path $currentDir -Filter "*.jpg" | sort-object -Descending -Property LastWriteTime -Top 1
Write-Verbose "Found image: [$BlogImage]"

#Rename and move image to blog location
$newImageName = Rename-Item -Path $BlogImage -NewName $blogImageName -PassThru
$newImagePath = $PlasterSplat.DestinationPath + "\assets\img\" + $plaster.Title.Replace(' ', '-').tolower()

if (Test-Path $newImagePath) {
    Move-Item $newImageName.FullName -Destination $newImagePath
} else {
    Write-Warning "Cannot find [$newImagePath]"
}

#End script info
Write-Verbose "SCRIPT NAME: $scriptName"
Write-Verbose "SCRIPT PATH: $currentDir"
Write-Verbose "SCRIPT RUNTIME: $datetime"
Write-Verbose "SCRIPT USER: $ScriptRunner"
Write-Verbose "SCRIPT SYSTEM: $compname.$domain"
```

# Conclusion
I used these scripts to write this latest blog post. It was rewarding to leverage PowerShell to help my workflow and remove any friction to writing new blog posts. While this is a very individual application of PowerShell I do hope you found something to inspire you to leverage it to help with everyday tasks.

# Learning More
* [**new_alainassaf_post** GitGHub repository](https://github.com/alainassaf/new_alainassaf_post)
* [**YouTube Recording of my RTPUG Presentation**](https://youtu.be/uiYuOab2B3w?si=1SNbH7WN1bF1Bnp0)
* [**Research Triangle PowerShell User Group**](https://rtpsug.com/)
* [**Ruby Programming Language**](https://www.ruby-lang.org/en/)
* [**Jekyll**](https://jekyllrb.com/)
* [**WSL**](https://learn.microsoft.com/en-us/windows/wsl/about)
* [**GitHub Pages**](https://pages.github.com/)
* [**Beautiful Jekyll**](https://beautifuljekyll.com/)
* [**Docker**](https://www.docker.com/)
* [**Develop GitHub Pages locally in a Ubuntu Docker Container by Bill Raymond**](https://youtu.be/zijOXpZzdvs?si=a16NvjmIYsEyOPCj)
* **Plaster**
    * [**Plaster Module**](https://github.com/PowerShellOrg/Plaster)
    * [**PowerShell: Adventures in Plaster (powershellexplained.com)**](https://powershellexplained.com/2017-05-12-Powershell-Plaster-adventures-in/)
    * [**PowerShell: GetPlastered a Plaster template to create a Plaster template (powershellexplained.com)**](https://powershellexplained.com/2017-05-14-Powershell-Plaster-GetPlastered-template/)
    * [**Working With Plaster — OverPoweredShell.com — By David Christian**](https://overpoweredshell.com/Working-with-Plaster/)

# Value for Value
If you received any value from reading this post, please [**buy me a coffee**](https://www.buymeacoffee.com/j72aXgIYJh){:target="_blank"} to show your support.
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="j72aXgIYJh" data-color="#16609f" data-emoji="☕"  data-font="Arial" data-text="Buy me a coffee" data-outline-color="#ffffff" data-font-color="#ffffff" data-coffee-color="#FFDD00" ></script>

*Thanks for Reading,*  
*Alain Assaf*