# Contributing to Thermo

## Overview
Welcome to the Thermo Repo! Here, you will find everything you need to start contributing to the Thermo Library. All you need is a GitHub account and the ability to explain how to solve a problem. If we accept your submission, our editors will review your content and direct you to make necessary changes before merging your content with the Library.

## What you need
* Expertise in your topic
* A tested, verified procedure or procedures for a topic related to Thermo
* A GitHub account and a fork of the Thermo repo.

## Table of contents
* Crucial criteria
* Payments
* Submission process
* Style guide
* Template
* Using images

## Crucial criteria
* **Accuracy:** Your procedures are straightforward, correct, and tested. For each action, include the “why” as well as the “what.” Explain the purpose behind each action.
* **Completeness:** Your guide produces a complete, functional configuration. When appropriate, your guide also gives readers of what to do next and provides all critical elements of security and other best practices.
* **Originality:** Anything that is not raw code is original content written exclusively for Thermo.

## Payments
We dispense all payments via PayPal. In most cases, we process payment within 30 days of publication, though in some cases in may take longer.
  Updates under 500 words (for example, CentOS 6 > 7) $100
  Simple guides, 500 - 999 words                      $200
  Complex guides, 1000+ words                         $300

## Submission process
Much of this process will be familiar to anyone familiar with GitHub. This section outlines the entire process, from your pull request (PR) to publication.

1. Submit your PR to the [thermo.io/docs master branch](https://github.com/thermoio/docs).
2. One or more subject matter experts (SMEs) reviews your PR and evaluates it for accuracy and completeness. They may direct your to make changes to your PR that must be resolved before proceeding to Step 3, or they may reject it.
3. The editor reviews your PR for adherence to our conventions and Style Guide, as well as checks it for accuracy, completeness, and originality. The editor may direct you to make further changes that must be resolved before proceeding to Step 4.
4. Our team conducts a final review. Upon acceptance, the editor sends you the IP Agreement in DocuSign.
5. Upon completion of the IP Agreement, our team merges your PR and publishes your submission to the Thermo Library.

## Style guide
We advise all contributors to review our Style Guide before submitting PRs; otherwise, our SMEs or editor will direct you to make such changes and prolong the effort.

**Attention:** All submissions must be in Markdown to be considered eligible for review.

### Active voice
Avoid passive voice; favor active voice. For example:

    “Save your progress to prevent data loss.”

        NOT

    “Progress must be saved or data will be lost.”

### Commands
* Use code blocks for all input and output, and designate the correct programming language. When presenting commands in a sentence, use `monospace font`. 
* When inserting code blocks in ordered lists, use the same indent as the copy in each step. For example:
  1. Install ModSec
     ```shell
     sudo yum install mod_security
     ```
  2. Restart Apache
     ```shell
     sudo service httpd restart
     ```
### Dates
Spell out all dates. For example:

    "November 15, 2018"
     
        NOT
     
    "11/15/18"
 
### File and folders
Use `monospace font` for all files, folders, paths, and directories.
 
### Format
See Templates.
 
### Headers
Use headline-style capitalization for the title; use sentence-style for all others. Use H1 for the title, H2 for sections, H3 for sub-sections, and so on.

### Images
Any content relating to a UI other than the CLI requires images. If the UI-related content involves a procedure, use an image for each step. See Using Images for more information.

### Language
As a general tip, please use direct, simple language. Omit needless words and phrases. 

### Lists
Use numbered lists for procedures. Break longer or complex procedures into sections with clear headers.

### Point of view
Use second-person language ("you"). Implied is favored in most cases. For example:

    "Click **Save** to continue."
    
        NOT
        
    "You'll want to click **Save** to continue."
    
## Template
(working)

## Using Images
Before uploading any images to the Thermo Repo:
1. Crop the image appropriately. Balance size with the need to guide the reader.
2. Make minimal but effective use of arrows, highlights, blurs, and other graphical tools to direct the reader. 

To embed an image in your article, you will create a file in the images folder in your fork, then submit this file along with your article in the PR.

1. In your fork, `<your_fork>/docs/images`, create a file that matches your article's name without the `.md` extension.
2. In that file, click **Upload files**, then upload the images.
3. In that same file, select the iamge, then click **Download**. Copy the URL in your browser, which you will use in Step 4 for `<URL_for_image_download>.
4. In your article, use the following Markdown, but replace the angled brackets (`<>`) with the URL you copied in Step 3.
```
