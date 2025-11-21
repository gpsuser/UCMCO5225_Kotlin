# Generate slides in ,md format from a lecture in .md format


## References note: 

Could include:

Microsoft (2025) Microsoft Copilot (AI assistant). Available at: https://www.microsoft.com (Accessed: 19 February 2025). The AI tool provided suggestions relating to formatting, grammar and content structuring that were reviewed and edited by the author, before inclusion in the final document.

---

## NOTE: - GENERATING THE .PPTX FILE
1. Once the md/marp version of the presentation is generated - use pandoc to convert it to .pptx format for final delivery to the user.

You can always open the final pptx file and eddit those slides that need to fit better visually.

The pandoc command is:

```bash

# -s = standalone - which means it includes all necessary preamble, where preamble means the necessary headers/footers for the pptx file
# -t pptx = output format is pptx

pandoc -s -t pptx input.md -o output.pptx

# or simply
pandoc input.md -o output.pptx

```
2. Note that you must create spaces between sentences and bullets so that the bullets show one line per bullet in the pptx file.

3. Insert slide numbers using powerpoiont menus - Insert - header/footer - Slide number - Apply to all

4. Manually format wach slide - quickly
---

## PROMPT

Please follow the steps and implement the requirements in the prompt that follows. Do not use any git tools for this task.

* **Role**

"
As an experienced teacher in `Kotlin programming`  - create a slide deck presentation in markdown format - based on the lecture content provided in the attached lecture file.
"

* **Task**

"
Read the attached `week5_refactor_lecture.md` lecture file  - to understand the content needed to create a slide deck presentation.

* Using this lecture content - create a slide deck in markdown format that can be downloaded as a .md file and is suitable for conversion into a slide deck using a tool such as Marp or similar.



"

* **Format**

"
* For diagrams use ascii  where appropriate. 
* Do not include learning outcomes on each slide
   
"

* **Tone**

"
Write in an explanatory teacher tone .
"

* **Audience**

"
The learners are undergraduate engineering students, in the UK.
"

* **Scope**

"

* The slide deck focuses entirely on delivering the lecture content component, of the lecture file.

* Do not include content in the slide deck that relates to: workshops, activities, assessments, tasks, case studies, etc. This will be covered separately.

* Please ensure that the content is well-organised and easy to follow - with time allocations for each new section.

* Make sure that the slide deck is visually appealing and engaging.

* Ensure that the slide deck is no more than `30` slides in total.

* The slide deck needs to be delivered in a `60` minute lecture.
* Make sure that the content is succinct and to the point, focussing on key topics - avoiding unnecessary detail. 

* examples - if any - need to be short and relevant to the lecture content (no more than 1 slide for an example).


"

* **Steps to include**

"
* Also include in the slide deck - one slide for each of the following:
  
  * table of contents: with section/topic, time allocation, learning outcome (LO1, LO2, etc.) - as a table 
  * objectives
  * learning outcomes
  * introduction
  * conclusion 

* Do not Number each slide - instead use titles/headings for each slide to indicate the content of the slide.  


"


<!-- * If the slide content is too long for a single slide (typically more than five bullet points)- please split the content into multiple slides. -->

<!-- * Include a "recap of previous lecture content" slide if a previous lecture is included in files in the context window. -->

<!-- * At the top of the markdown slide deck include instructions for how to convert the markdown file into a slide deck using a tool such as Marp or similar - using the following format:

``` markdown

---
marp: true
theme: default
paginate: true
---

``` -->