# Objective 0 - Logging in & Navigating the AWS Console Page

1) Find your credential email with subject: **NCBI Codethon Credentials**

![img1](doc_images/img1.jpg)

2) Navigate to [https://codeathon.ncbi.nlm.nih.gov](https://codeathon.ncbi.nlm.nih.gov) and login using the credentials from the email *(left image)* and create a new password after logging in *(right image)*

![img2](doc_images/img2.jpg)


3) Click "Sign-In" under the AWS Console Sign-In column on the new page

![img3](doc_images/img3.jpg)

4) If you see **AWS Management Console** like the screenshot below, you have successfully logged in to the AWS Console!

![img4](doc_images/img4.jpg)

> **NOTE:** If you are logged out or get kicked out of the console, simply return to [https://codeathon.ncbi.nlm.nih.gov](https://codeathon.ncbi.nlm.nih.gov) and log in again (remember your newly created password!) to get back to the console home page.

> **NOTE:** This login method is unique to the workshop. If you want to create your own account after the workshop, visit the link below and follow the steps:
> [https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

# Objective 1 - Search for the sequence reads deposited into NCBI's SRA database using AWS Athena

## Creating an S3 Bucket

Before we can use Athena, we need to make an S3 bucket that we can save our results to as we search the SRA metadata tables. So, let's go make one!

1) Use the search bar at the top of the console page to search for **S3** and click on the first result

![img5](doc_images/img5.jpg)

2) Click the orange **Create Bucket** button on the right-hand side of the screen

![img6](doc_images/img6.jpg)

3) Enter a bucket name and make sure the region is set to `US East (N. Virginia) us-east-1`  
**S3 bucket names must be completely unique.**  
For this workshop, use the format `<username>-cloud-workshop` where `<username>` is the username you used to login

![img7](doc_images/img7.jpg)

4) Scroll down to the **Block Public Access settings for this bucket** section. Deselect the blue checkbox at the top and select the bottom 2 checkboxes. Then check the box underneath the warning symbol to acknowledge your changes to the public access settings

![img8](doc_images/img8.jpg)

5) Ignore the rest of the settings and scroll to the bottom of the page. Click the orange **Create Bucket** button.

![img9](doc_images/img9.jpg)

> For more information about the settings we skipped, visit the supplementary text (section: **In-depth S3 bucket creation**)

6) Clicking the button will redirect you back to the main S3 page. You should be able to find your new bucket in the list. If so, you have successfully created an S3 bucket!

![img10](doc_images/img10.jpg)

Now that we have an S3 bucket ready, we can go see what the Athen page looks like!

## Navigating to Athena

1) Use the search bar at the top of the console page to search for **Athena** and click on the first result

![img11](doc_images/img11.jpg)

2) Visiting the Athena page should prompt you with a few notifications at the top of the page. you can just click the little "X" and disregard the first two.

![img12](doc_images/img12.jpg)

3) The third notification is actually very important .We need to tell Athena which S3 bucket we want all of our search results to be stored in. Good thing we just made one, eh?  Click **Settings** in the top right of the screen

![img13](doc_images/img13.jpg)

4) Click the folder button that says **Select** *(top image)* and scroll down to find your newly created S3 bucket. Then click the black arrow button to select it and the blue **Select** button to add it *(middle image)*. Finally, click **Save** *(bottom image)*.

![img14](doc_images/img14.jpg)

![img15](doc_images/img15.jpg)

![img16](doc_images/img16.jpg)

Now that we can save Athena results we can run some searches! The very last step to doing that is to import the table we want to search into Athena using another AWS service - Glue. However, to save time, this has already been done prior to the workshop by the instructor.

> For the detailed instructions on using AWS Glue to add a table to Athena, visit the Supplementary Text (section: **Instructions for AWS Glue**)!

## Exploring Athena Tables

These steps aren't necessary to do before every Athena query, but they are useful when exploring a new table.

1) Click the dropdown menu underneath the **Database** section and click **sra** to set it as the active database. If you do not see this as an option, refresh the page and check again.

![img17](doc_images/img17.jpg)

2) Look at the **Tables** section and click the ellipses next to the **metadata** table, then click **Preview Table** to automatically run a sample command which will give you 10 random lines from the table

![img18](doc_images/img18.jpg)

> For SRA based tables, you can also visit the following link to get the definition of each column in the table: [https://www.ncbi.nlm.nih.gov/sra/docs/sra-cloud-based-examples/](https://www.ncbi.nlm.nih.gov/sra/docs/sra-cloud-based-examples/)

## Querying Our Dataset

1) The following link is the actual publication for our case study today. Scroll to the very bottom and find the **Data Availability** section: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5778042/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5778042/)

![img19](doc_images/img19.jpg)

> You can also use the SRA Run Selector on the NCBI website to download data from NCBI directly to your S3 bucket! Find more information and a tutorial here:  
> [https://www.ncbi.nlm.nih.gov/sra/docs/data-delivery/](https://www.ncbi.nlm.nih.gov/sra/docs/data-delivery/)

2) The paper stored the data we need under and ID **SRP125431**, but we don't know exactly which column that is associated with. So, scroll through the preview table we made earlier to find a column filled with similar values.

![img20](doc_images/img20.jpg)

> The **Preview Table** query we used to make this example pulls random lines from the table, so the values within your table may look different from this screenshot. The important info for us is that each value in this **sra_study** column starts with **"SRP"**

3) Now that we know which column to query for our data (sra_study), we can build the Athena query. Look to the panel where we enter our Athena queries. Click **New Query 1** to navigate back to the empty panel so we can write our own query.

![img21](doc_images/img21.jpg)

> Fun fact: If you navigate back to **New Query 2** you should still see the result table for that query! Athena will save that view for you until you run a new query in that tab or close the webpage.

4) Type the following command (circled in yellow) into your empty query space, then click the blue **Run Query** button (circled in red).

![img22](doc_images/img22.jpg)

5) If you see a results table with three rows like the partial screenshot below, you have successfully found your data!

![img23](doc_images/img23.jpg)

6) Click the **Download to CSV** button on the top-right corner of the results panel to download your file to your computer. You should be able to open this in Microsoft Excel, Google Sheets, or a regular text editor (e.g., Notepad for PC, TextEdit for Mac). We will review this file later, so keep it handy.

![img24](doc_images/img24.jpg)

> Want to challenge yourself? Visit the supplementary text (section: **SQL challenges**) to find some questions you can build your own SQL query for. Plus, find more advanced SQL query techniques and a deeper breakdown of the SRA metadata table too!

# Objective 2 - Aligning Sequence Reads using AWS EC2 & MagicBLAST

## Launching an EC2 Instance

1) Use the search ar at the top of the console page to search for **EC2** and click on the first result

![img25](doc_images/img25.jpg)
