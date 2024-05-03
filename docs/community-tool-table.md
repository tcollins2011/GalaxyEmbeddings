---
layout: tutorial_hands_on
title: Creation of an interactive Galaxy tools table for your community
level: Introductory
subtopic: tooldev
questions:
- Is it possible to have an overview of all Galaxy tools for a specific scientific domain?
- How can I create a new overview for a specific Galaxy community or domain?
objectives:
- Create a community reviewed table for Galaxy tools within a specific scientific domain
- Embed an interactive table in a community page
time_estimation: 1H
key_points:
- The Galaxy Tool Metadata Extractor extracts all Galaxy tools to create interactive tables
- The tool tables can be filtered by ToolShed categories and community-reviewed lists of tools to keep or exclude
- The community interactive Galaxy tools table can be embed into any website
tags:
- Community
- SIG
contributions:
  authorship:
    - bebatut

---

Galaxy offers thousands of tools. They are developed across various GitHub repositories. Furthermore, Galaxy also embraces granular implementation of software tools as sub-modules. In practice, this means that tool suites are separated into Galaxy tools, also known as wrappers, that capture their component operations. Some key examples of suites include [Mothur](https://bio.tools/mothur) and [OpenMS](https://bio.tools/openms), which translate to tens and even hundreds of Galaxy tools. 

While granularity supports the composability of tools into rich domain-specific workflows, this decentralized development and sub-module architecture makes it **difficult for Galaxy users to find and reuse tools**. It may also result in Galaxy tool developers **duplicating efforts** by simultaneously wrapping the same software. This is further complicated by a lack of tool metadata, which prevents filtering for all tools in a specific research community or domain, and makes it all but impossible to employ advanced filtering with ontology terms and operations like [EDAM ontology](https://edamontology.org/page). 

The final challenge is also an opportunity: the global nature of Galaxy means that it is a big community. Solving the visibility of tools across this ecosystem and the potential benefits are far-reaching for global collaboration on tool and workflow development.

To provide the research community with a comprehensive list of available Galaxy tools, [Galaxy Tool Metadata Extractor](https://github.com/galaxyproject/galaxy_tool_metadata_extractor) was developed to collect Galaxy wrappers from a list of Git repositories and automatically extract their metadata (including Conda version, [bio.tools](https://bio.tools/) identifiers, and EDAM annotations). The workflow also queries the availability of the tools and usage statistics from the three main Galaxy servers (usegalaxy.*). 

![A diagram illustrating the Galaxy Tool Metadata Extractor pipeline, showcasing the various steps involved in creating a community Galaxy tool table.](./images/galaxy_tool_metadata_extractor_pipeline.png "Workflow of the Galaxy Tool Metadata Extractor pipeline. Tool wrappers are parsed from different repositories and additional metadata is retrieved from bio.tools, BioConda, and the main public Galaxy servers. Upon filtering and manual curation of the data for specific scientific communities, the data is transformed into interactive web tables and a tool usage statistic-base word cloud, that can be integrated into any website.")

The pipeline creates an [interactive table with all tools and their metadata](https://galaxyproject.github.io/galaxy_tool_metadata_extractor/). This table can be **filtered to only include tools that are relevant to a specific research community**. Here is an example for the microbial related tools:

<iframe id="edam" src="https://galaxyproject.github.io/galaxy_tool_metadata_extractor/microgalaxy/" frameBorder="0" width="100%" height="600px"> ![Interactive table for microgalaxy tools](./images/microgalaxy_tools.png) </iframe>

The generated community-specific interactive table can be used as it and/or embedded, e.g. into the respective Galaxy Hub page or Galaxy subdomain. This table allows further filtering and searching for fine-grained tool selection. 

The pipeline is **fully automated** and executes on a **weekly** basis. Any research community can apply the pipeline to create a table specific to their community. 

The aim is this tutorial is to create such table for a community.

> <agenda-title></agenda-title>
>
> In this tutorial, we will cover:
>
> 1. TOC
> {:toc}
>
{: .agenda}

# Add your community to the Galaxy Tool Metadata Extractor pipeline

To create a table for a community, you first need to create a new folder in the `data/community` folder within [Galaxy Tool Metadata Extractor code source](https://github.com/galaxyproject/galaxy_tool_metadata_extractor). 

> <hands-on-title>Create a folder for your community</hands-on-title>
>
> 1. If not already done, fork the [Galaxy Tool Metadata Extractor repository](https://github.com/galaxyproject/galaxy_tool_metadata_extractor) 
> 2. Go to the `data/communities` folder
> 3. Click on **Add file** in the drop-down menu at the top
> 4. Select **Create a new file**
> 5. Fill in the `Name of your file` field with:  name of your community + `/categories`
>
>    This will create a new folder for your community and add a categories file to this folder.
> 
{: .hands_on}

One of the filters for the main community table is based on the tool categories on the [Galaxy ToolShed](https://toolshed.g2.bx.psu.edu/). Only tools in the selected ToolShed categories will be added to the filtered table. As a result, it is recommended to include broad categories.

> <hands-on-title>Select the ToolShed categories</hands-on-title>
>
> 1. Go to the [Galaxy ToolShed](https://toolshed.g2.bx.psu.edu/)
> 2. On the main page, pick the most obvious categories that represent tools used by your community
> 3. Add the name of these categories in the `categories` file you started above, with 1 ToolShed category per row
>
>    For example:
>    ```
>    Assembly
>    Metagenomics
>    ```
>
> 4. Search on the [Galaxy ToolShed](https://toolshed.g2.bx.psu.edu/) for some of the popular tools in your community
> 5. Open the tool entries on the ToolShed, and note their categories
> 6. Add any new categories to the `categories` file 
{: .hands_on}

Once you have a list of the ToolShed categories that you wish to keep, you can submit this to Galaxy Tool Metadata Extractor.

> <hands-on-title>Submit the new community to Galaxy Tool Metadata Extractor</hands-on-title>
>
> 1. Click on **Commit changes** at the top
> 2. Fill in the commit message with something like `Add X community`
> 3. Click on `Create a new branch for this commit and start a pull request`
> 4. Create the pull request by following the instructions
> 
{: .hands_on}

The Pull Request will be reviewed. Make sure to respond to any feedback. 

Once the Pull Request is merged, the interactive table will be created and made available online at `https://galaxyproject.github.io/galaxy_tool_metadata_extractor/<your community>`.

# Review the generated interactive table

The interactive table will contain all the tools associated with the ToolShed categories that you selected. However, not all of these tools might be interesting for your community. 

Galaxy Tool Metadata Extractor provides 2 optional filters for tools:
- A list of tools to exclude
- A list of tools that should definitely be kept.

## Add a list of tools to exclude

> <hands-on-title>Add a list of tools to exclude</hands-on-title>
>
> 1. Open your interactive table
> 2. Search for some tools to exclude
> 3. Go to your community folder on [Galaxy Tool Metadata Extractor repository](https://github.com/galaxyproject/galaxy_tool_metadata_extractor)
> 4. Click on **Add file** drop-down menu on the top
> 5. Select **Create a new file**
> 6. Fill in `tools_to_exclude` in `Name of your file`
> 7. Add the `Galaxy wrapper id` of the tools to exclude, with 1 tool identifier per row
> 8. Submit your changes as before
> 9. Wait for the Pull Request to be merged
>
{: .hands_on}

## Add a list of tools to keep

> <hands-on-title>Add a list of tools to keep</hands-on-title>
>
> 1. Open your interactive table
> 2. Search for some tools to keep
> 3. Go to your community folder on [Galaxy Tool Metadata Extractor repository](https://github.com/galaxyproject/galaxy_tool_metadata_extractor)
> 4. Click on **Add file** drop-down menu on the top
> 5. Select **Create a new file**
> 6. Fill in `tools_to_keep` in `Name of your file`
> 7. Add the `Galaxy wrapper id` of the tools to keep, with 1 tool identifier per row
> 8. Submit your changes as before
> 9. Wait for the Pull Request to be merged
>
{: .hands_on}

## Review all tools in your table

Once the required filters have been implemented, you can review the full list of tools to ensure that you have created an interactive table reflecting the Galaxy tool landscape for your community.

> <hands-on-title>Review all tools in your table</hands-on-title>
>
> 1. Download the `tools.tsv` file in `results/<your community>`
> 2. Open `tools.tsv` with a Spreadsheet Software
> 3. Review each line corresponding to a tool
>    1. Change the value in the `Reviewed` column from `False` to `True`
>    2. Add `True` to the `To keep` column if the tool should be kept, and `False` if not
> 4. Extract the list of tools to keep
>    1. Create a sheet for the list of tools to keep to
>       1. Filter on the `To keep` column to keep value `True`
>       2. Keep only the `Galaxy wrapper id` column
>    2. Add this list of `Galaxy wrapper id` to the `tools_to_keep` file
> 5. Extract the list of tools to exclude
>    1. Create a sheet for the list of tools to keep to
>       1. Filter on the `To keep` column to keep value `False`
>       2. Keep only the `Galaxy wrapper id` column
>    2. Add this list of `Galaxy wrapper id` to the `tools_to_exclude` file
> 6. Submit the changes
> 7. Wait for the Pull Request to be merged
>
{: .hands_on}

You should have now an interactive table reflecting the Galaxy tool landscape for your community.

# Embed the interactive table in your community page on the Hub

The interactive table you have created can be embedded in your community page on the Hub, e.g. [microGalaxy](https://galaxyproject.org/community/sig/microbial/#tools).

> <hands-on-title>Embed your table as an iframe</hands-on-title>
>
> 1.  If not already done, fork the repository [Galaxy Hub](https://github.com/galaxyproject/galaxy-hub)
> 2. Open or create your community page: `content/community/sig/<your community>/index.md`
> 3. Add an iframe to embed the interactive table
>
>    ```
>    <iframe
>      id="inlineFrameExample"
>      title="Microbial related tools"
>      width="100%"
>      height="600"
>      frameBorder="0"
>      src="https://galaxyproject.github.io/galaxy_tool_metadata_extractor/<your_community>/">
>    </iframe>
>    ```
>
> 4. Replace `<your_community>` by the name of your community in `src`
> 5. Submit the changes
> 7. Wait for the Pull Request to be merged
>
{: .hands_on}

# Conclusion

You now have an interactive table with Galaxy tools available for your community, and this table is embedded in a community page.

