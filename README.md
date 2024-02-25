# Introduction to Quarto

*Quarto* is an open-source scientific and technical publishing system. It aims to making knowledge sharing more dynamic, more accessible and more reproducible through a set of workflows and best practices. My aim is to use *Quarto* as an orchestration tool for knowledge repos. This repo will be a starting template for documenting my projects and area of interests.

> Analyze. Share. Reproduce. You have a story to tell with data — Tell it with Quarto.

## Basic Concepts

Mostly *Quarto* recommends using `.qmd` file to create a document but there are options to document your code within `.ipynb` notebook using the [VS Code Notebook Editor](https://quarto.org/docs/tools/vscode.html#notebook-editor) as well as using the [JupyterLab](https://quarto.org/docs/tools/jupyter-lab.html). The latter will provide a complete workflow to document projects related to software development and data science.

### Documentation

There are several types of documentation that can be created using *Quarto*. These are listed as followed:

* [Document](https://quarto.org/docs/get-started/authoring/) — The basic component which contains only a `.qmd` file or `.ipynb` notebook. This can be published like a single page website.
* [Websites](https://quarto.org/docs/websites/) — Publish collections of documents as a website. Websites support multiple forms of navigation and full-text search.
* [Blogs](https://quarto.org/docs/websites/website-blog.html) — Create a blog with an about page, flexible post listings, categories, RSS feeds, and over twenty themes.
* [Presentations](https://quarto.org/docs/presentations/) — Author PowerPoint, Beamer, and Revealjs presentations using the same syntax you’ve learned for creating documents.
* [Books](https://quarto.org/docs/books/) — Create books and manuscripts in print (PDF, MS Word) and online (HTML, ePub) formats.
* [Interactivity](https://quarto.org/docs/interactive/) — Include interactive components to help readers explore the concepts and data you are presenting more deeply.

To be more reproducible and more organized, *Quarto* should not be used as a single **Document** even if there is a single document file. Instead, publishing collections of it in a **Websites** style, it is better documented and more suitable for publishing online.

## Quarto Workflows

To successfully orchestrate sharable documents using *Quarto*, you need to have a good workflow and in order to have that we need to first understand components and commands that *Quarto* has.

### Overview

Basic *Quarto* workflow is simply these 3 steps:

```mermaid
flowchart LR
  A[Authoring] --> B[Rendering] --> C[Publishing]
```

Let's break down what we have to do in each step by

1. **Authoring** — Usually this step is an iterative approach. You can imagine it like you are in a development phase of your project. What you have to do is code execuation or more specifically:
    
    > Code Commit then Repeat.
  
    The difference when you have *Quarto* is you need to add some documentation along the way while you developing your code. Therefore, you will need to see changes in both of your code and documentation version of it. To do that using the command below.

    ```bash
    # Preview the whole project in the current directory
    quarto preview
    ```

    In order to preview a report for an individual file.

    ```bash
    # Preview only for an index file
    quarto preview index.ipynb
    ```

    What *Quarto* does behind the scene is that it renders files and watches those for any changes. Some output files may be generated such as `.quarto` and `_site` folder in case of a website project. Those files are the way *Quarto* uses to keep track of your project (code, outputs and documentation) and be able to provide a live preview for your development.
  
    Note that it is best practice to have a version control service such as [GitHub](https://github.com/) to keep track of your code by the beginning of your project. In addition, you should add a `.gitignore` file contains both `.quarto` and `_site` if you want to use *Quarto* to render and publish those contents.

    Moreover, *Quarto* uses a `_quarto.yml` file to configure metadata about your project. For example, if your project is a website, `_quarto.yml` may contain information about a title or theme. Also, it can be the place where configurations on rendering files are.

    In sum, there are processes within authoring itself — **Execution**, **Preview** and **Project Metadata**.

2. **Rendering** — This process is about generating a report-like document for each executable files. A fully re-rendering is requrired as to have all changes reflected before publishing.

    ```bash
    # Render the whole project in the current directory
    quarto render
    ```

    In order to render only an individual file.

    ```bash
    # Render only for an index file
    quarto render index.ipynb
    ```

    Note that *Quarto* has rendered targets by default to be only executable files listed here: `.qmd`, `,ipynb`, `.md` and `.Rmd`. See the [Render Targets](https://quarto.org/docs/websites/#render-targets) configuration.

    If you want to execute a report on-the-fly. Use `--execute` flag.

    ```bash
    # Execute and render the whole project in the current directory
    quarto render --execute
    
    # Execute and render only an index file
    quarto render index.ipynb --execute
    ```

    Note that if you are using `--execute` flag and `_quarto.yml` is set to have something like [Freeze Option](https://quarto.org/docs/projects/code-execution.html#freeze), a `_freeze` directory will be created automatically to store the results of computations. Also, these results should be check in to version control.
    
    But What is `freeze`? — It is an option to denote that computational documents should be re-rendered. When `freeze` is set to `auto` or `true`, you do not to fully re-render the code or just re-render as needed. This makes your workflow more efficient.
    
    Anyway, in ML/DL projects, most of the time, you do not want to re-execute the code as it has already done by another cloud services like [Google Colab](https://colab.google/), [Kaggle](https://www.kaggle.com/) or [Amazon EC2](https://aws.amazon.com/ec2/) for faster training the models. So, the `--execute` flag will not be used and `_freeze` directory will never be created.
    
    Then What happens if we do not have the computation results in `_freeze`? — My understanding is all necessary files are still rendered in `_site` and `.quarto` and when you publish your project even `_site` and `.quarto` are ignored by a `.gitignore` file, Under the hood, *Quarto* is able to use those rendered files to be publish on the hosting service without depending on `_freeze`.

    In sum, we do not need to pay attention to a `_freeze` directory as we still have rendered files in `_site` and `.quarto`. But, we can not ignore setting a `freeze` option to `auto` in `_quarto.yml` because it is important for efficint-sake that we are re-rendering only if source code changes.

3. **Publishing** — This is another iterative step. It may required you to deploy your websites or documents several times before reaching the desired output. In my workflow, I will use [GitHub Pages](https://pages.github.com/) as a hosting service.
    
    All the steps are provided in [Publishing with GitHub Pages](https://quarto.org/docs/publishing/github-pages.html) but I will point out some interesting sections.

    If you are not using Render to `docs` method to publish your website. You will need to publish using *Quarto*.

    ```bash
    # Publish on gh-pages branch
    quarto publish gh-pages
    ```

    ```bash
    # Publish a single document file
    quarto publish gh-pages index.ipynb
    ```

    Note that you will need to follow these two steps before hand:
    
    * Check out and push `gh-pages` branch to create a source branch.
    * Set up `gh-pages` as a source branch for *GitHub Pages* in **Settings** : **Pages**.

All the steps are repeating as the development continues. It is also updating the project as a **release** while the source code changes along the way.

Using help for *Quarto* commands.

```bash
# Documetation for Quarto commands
quarto --help
```

### CI/CD with GitHub Actions

Now, we manually publish the website to *GitHub Pages* but what if we want to remove this tedious process of running `quarto publish gh-pages`, [GitHub Actions](https://docs.github.com/en/actions) is what you want. It will automate integration processes like code checkout, running tests and building/packaging your code through `.github/workflows`. In other words, it will publish your website automatically triggered from commits.

Note that you also need to change **Workflow permissions** in the **Actions** section of your repository **Settings** to be **Read and write permission** in order to allow *GitHub Actions* to trigger the workflow.

Depending on how much you want **GitHub Actions** to automate your workflow. You are free to modify `.github/workflows/publish.yml` as you like. Now, I set up an automation to just **Rendering** and **Publishing** not re-executing the code. This is because most ML/DL projects are not feasible to be executing on CI/CD server! See the guide to [Publishing with Continuous Integration (CI)](https://quarto.org/docs/publishing/ci.html) to choose the one the fit your need.

## References

* **Markdown Syntax and Tools**
    * [Markdown Basic Syntax](https://www.markdownguide.org/basic-syntax/)
    * [Mermaid Diagramming and Charting Tool](https://mermaid.js.org/intro/)

* **Quarto Documentation**
    * [How to get started with Quarto](https://quarto.org/docs/get-started/)
    * [Comprehensive guide to using Quarto](https://quarto.org/docs/guide/)
    * [GitHub Actions for Quarto](https://github.com/quarto-dev/quarto-actions/)

* **Quarto Tutorials and Examples**
    * [Making Sharable Documents with Quarto](https://openscapes.github.io/quarto-website-tutorial/)
