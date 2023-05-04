Download Link: https://assignmentchef.com/product/solved-csde502-homework9
<br>
<em><strong>Explanation</strong></em>: This assignment is intended to give you more practice delving into the Add Health data set and in manipulating additional variables.

<em><strong>Instructions</strong></em>:

<ol>

 <li>Make sure your Rmd file has no local file system dependencies (i.e., anyone should be able to recreate the output HTML using only the Rmd source file).</li>

 <li>Make a copy of this Rmd file and add answers below each question. The code that generated the answers should be included, as well as the complete source code for the document.</li>

 <li>Change the YAML header above to identify yourself and include contact information.</li>

 <li>For any tables or figures, include captions and cross-references and any other document automation methods as necessary.</li>

 <li>Make sure your output HTML file looks appealing to the reader.</li>

 <li>Upload the final Rmd to your github repository.</li>

 <li>Download <a href="https://staff.washington.edu/phurvitz/csde502_winter_2021/assignments/assn_id.txt">assn_id.txt</a> and include the URL to your Rmd file on github.com.</li>

 <li>Create a zip file from your copy of assn_id.txt and upload the zip file to the Canvas site for Assignment 9. <em><strong>The zip file should contain only the text file. Do not include any additional files in the zip file–everything should be able to run from the file you uploaded to github.com. Please use zip format and not 7z or any other compression/archive format.</strong></em></li>

</ol>

<h1>1</h1>

<strong>Using the full household roster (you’ll need to go back the full raw data source, <a href="https://staff.washington.edu/phurvitz/csde502_winter_2021/data/21600-0001-Data.dta.zip">21600-0001-Data.dta</a>), create the following variables for each respondent. Document any decisions that you make regarding missing values, definitions, etc. in your narrative as well as in the R code. Include a frequency tabulation and a histogram of each result.</strong>

Starting by pulling in the full dataset from GitHub and listing the variables.

add_helth &lt;- haven::read_dta(“https://github.com/dmccoomes/csde502_winter_2021_dcoomes/raw/main/Homework/homework_09/data/21600-0001-Data.dta”) metadata &lt;- bind_cols(    # variable name    varname = colnames(add_helth),    # label    varlabel = lapply(add_helth, function(x) attributes(x)$label) %&gt;%         unlist(),    # format    varformat = lapply(add_helth, function(x) attributes(x)$format.stata) %&gt;%        unlist(),    # values    varvalues = lapply(add_helth, function(x) attributes(x)$labels) %&gt;%         # names the variable label vector        lapply(., function(x) names(x)) %&gt;%         # as character        as.character() %&gt;%         # remove the c() construction        str_remove_all(“^c\(|\)$”)) DT::datatable(metadata)

<h2>1.1</h2>

<strong>Total number in household</strong>

I will use the question “How many people live in household?” to construct the total number in the household. I will not include any observations that reported they don’t live in a regular household. As we can see from <strong>Table 1</strong> and <strong>Figure 1</strong> more households have 4 members as compared to other numbers, and the distribution of those that answered is right-skewed.

add_helth %&lt;&gt;%  mutate(num_house=S27) %&gt;%  mutate(num_house=ifelse(num_house==7|num_house==99, NA, num_house))add_helth %&gt;%  group_by(num_house) %&gt;%  summarize(n=n()) %&gt;%  mutate(`%`=n/sum(n)*100) %&gt;%  mutate(`%`=`%` %&gt;% round(1)) %&gt;%  mutate(“cum %”= round(cumsum(n/sum(n)*100), 1)) %&gt;%  kable(caption=”Total number of individuals living in household”) %&gt;%  kable_styling(full_width=FALSE, position=”left”,                 bootstrap_options = c(“striped”, “hover”))

<table>

 <thead>

  <tr>

   <td colspan="4">Table 1.1: Total number of individuals living in household</td>

  </tr>

  <tr>

   <td><strong>num_house </strong></td>

   <td><strong>n </strong></td>

   <td><strong>% </strong></td>

   <td><strong>cum % </strong></td>

  </tr>

 </thead>

 <tbody>

  <tr>

   <td>1</td>

   <td>24</td>

   <td>0.4</td>

   <td>0.4</td>

  </tr>

  <tr>

   <td>2</td>

   <td>239</td>

   <td>3.7</td>

   <td>4.0</td>

  </tr>

  <tr>

   <td>3</td>

   <td>853</td>

   <td>13.1</td>

   <td>17.2</td>

  </tr>

  <tr>

   <td>4</td>

   <td>1564</td>

   <td>24.0</td>

   <td>41.2</td>

  </tr>

  <tr>

   <td>5</td>

   <td>1095</td>

   <td>16.8</td>

   <td>58.0</td>

  </tr>

  <tr>

   <td>6</td>

   <td>822</td>

   <td>12.6</td>

   <td>70.7</td>

  </tr>

  <tr>

   <td>NA</td>

   <td>1907</td>

   <td>29.3</td>

   <td>100.0</td>

  </tr>

 </tbody>

</table>

bins &lt;- length(unique(add_helth$num_house))-1 ggplot(data=add_helth, mapping=aes(x=num_house)) +  geom_histogram(bins=bins, color=”red”, fill=”white”) +   theme_bw() +   labs(x=”Number of people per household”, y=”Count”)

Figure 1.1: Histogram of the number of people per household

<h2>1.2</h2>

<strong>Number of sisters</strong>

<h2>1.3</h2>

<strong>Number of brothers</strong>

<h2>1.4</h2>

<strong>Total number of siblings</strong>

<h1>2</h1>

<strong>What proportion of students live with two biological parents? Include the analysis in your R code.</strong>

<h1>3</h1>

<strong>Calculate the number of household members that are NOT biological mother, biological father, full brother or full sister. Create a contingency table and histogram for this variable.</strong>

<h2>3.1 Source code</h2>

cat(readLines(con = “dcoomes_hw_09.Rmd”), sep = ‘
’)—title: “CSDE 502 Winter 2021, Assignment 8″author: “[dcoomes](mailto:<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d1b5b2bebebcb4a291a4a6ffb4b5a4">[email protected]</a>)”output:     bookdown::html_document2:        number_sections: true        self_contained: true        code_folding: hide        toc: true        toc_float:            collapsed: true            smooth_scroll: false    pdf_document:        number_sections: true        toc: true        fig_cap: yes        keep_tex: yesurlcolor: blue — “`{r, warning=FALSE, message=FALSE} knitr::opts_chunk$set(echo = TRUE, warning=FALSE, message = FALSE) library(captioner)library(tidyverse)library(magrittr)library(kableExtra) figure_nums &lt;- captioner(prefix = “Figure”)table_nums &lt;- captioner(prefix = “Table”)“` ___Explanation___:This assignment is intended to give you more practice delving into the Add Health data set and in manipulating additional variables.  ___Instructions___:  1. Make sure your Rmd file has no local file system dependencies (i.e., anyone should be able to recreate the output HTML using only the Rmd source file).1. Make a copy of this Rmd file and add answers below each question. The code that generated the answers should be included, as well as the complete source code for the document.1. Change the YAML header above to identify yourself and include contact information.1. For any tables or figures, include captions and cross-references and any other document automation methods as necessary.1. Make sure your output HTML file looks appealing to the reader.1. Upload the final Rmd to your github repository.1. Download [`assn_id.txt`](http://staff.washington.edu/phurvitz/csde502_winter_2021/assignments/assn_id.txt) and include the URL to your Rmd file on github.com.1. Create a zip file from your copy of `assn_id.txt` and upload the zip file to the Canvas site for Assignment 9. ___The zip file should contain only the text file. Do not include any additional files in the zip file–everything should be able to run from the file you uploaded to github.com. Please use zip format and not 7z or any other compression/archive format.___  #__Using the full household roster (you’ll need to go back the full raw data source, [21600-0001-Data.dta](http://staff.washington.edu/phurvitz/csde502_winter_2021/data/21600-0001-Data.dta.zip)), create the following variables for each respondent. Document any decisions that you make regarding missing values, definitions, etc. in your narrative as well as in the R code.  Include a frequency tabulation and a histogram of each result.__ Starting by pulling in the full dataset from GitHub and listing the variables. “`{r, cache=TRUE, results=’hide’} add_helth &lt;- haven::read_dta(“https://github.com/dmccoomes/csde502_winter_2021_dcoomes/raw/main/Homework/homework_09/data/21600-0001-Data.dta”) metadata &lt;- bind_cols(    # variable name    varname = colnames(add_helth),    # label    varlabel = lapply(add_helth, function(x) attributes(x)$label) %&gt;%         unlist(),    # format    varformat = lapply(add_helth, function(x) attributes(x)$format.stata) %&gt;%        unlist(),    # values    varvalues = lapply(add_helth, function(x) attributes(x)$labels) %&gt;%         # names the variable label vector        lapply(., function(x) names(x)) %&gt;%         # as character        as.character() %&gt;%         # remove the c() construction        str_remove_all(“^c\(|\)$”)) DT::datatable(metadata) “` ##__Total number in household__ I will use the question “How many people live in household?” to construct the total number in the household. I will not include any observations that reported they don’t live in a regular household. As we can see from **`r table_nums(name=”numtable”, display=”cite”)`** and **`r figure_nums(name=”numhist”, display=”cite”)`** more households have 4 members as compared to other numbers, and the distribution of those that answered is right-skewed. “`{r} add_helth %&lt;&gt;%  mutate(num_house=S27) %&gt;%  mutate(num_house=ifelse(num_house==7|num_house==99, NA, num_house)) “` “`{r numtable} add_helth %&gt;%  group_by(num_house) %&gt;%  summarize(n=n()) %&gt;%  mutate(`%`=n/sum(n)*100) %&gt;%  mutate(`%`=`%` %&gt;% round(1)) %&gt;%  mutate(“cum %”= round(cumsum(n/sum(n)*100), 1)) %&gt;%  kable(caption=”Total number of individuals living in household”) %&gt;%  kable_styling(full_width=FALSE, position=”left”,                 bootstrap_options = c(“striped”, “hover”))  “` “`{r numhist, fig.cap=”Histogram of the number of people per household”} bins &lt;- length(unique(add_helth$num_house))-1 ggplot(data=add_helth, mapping=aes(x=num_house)) +  geom_histogram(bins=bins, color=”red”, fill=”white”) +   theme_bw() +   labs(x=”Number of people per household”, y=”Count”) “` ##__Number of sisters__  ##__Number of brothers__  ##__Total number of siblings__  #__What proportion of students live with two biological parents? Include the analysis in your R code.__  #__Calculate the number of household members that are NOT biological mother, biological father, full brother or full sister. Create a contingency table and histogram for this variable.__ ## Source code“`{r comment=”}cat(readLines(con = “dcoomes_hw_09.Rmd”), sep = ‘
’)“`


