
  #Homework 2
   *Question 1:* Look at the head and the tail of chipotle.tsv in the data subdirectory of this repo. Think for a minute about how the data is structured. What do you think each column means? What do you think each row means? Tell me! (If you're unsure, look at more of the file contents.)

    The Order ID is a unique ID used to keep track of each item that is part of the overall order. There are multiple instances of the same Order ID, because each line represents an individual item in the order

    The quanitity represents the total number of items ordered for one type of item; it does not represent the total number of items in ordered in the order

    The item_name represents the type of item ordered. From an inventory perspective, the item_name is not unique, as there are sub-categories

    The item_description provides an itemized list of what was specifically ordered. This is needed for both cost and inventory accounting.

    The item_price is the total cost of an item_name ordered

    *Question 2:* How many orders do there appear to be?

    I had to do some online research to come to this conclusion. I used the "cut" command to focus on field/tab1. Then I piped this with the "sort" and "unique" to sort and de-dup the orders in column 1. Finally, piped this wc -1, to count the remaining lines.

    cut -f1 chipotle.tsv | sort | unique | wc -1

    Total number of unique order numbers: 1835

  *Question 3:* How many lines are in this file?

    wc -1 chipotle.tsv

    Total number of lines: 4623

    *Question 4:* Which burrito is more popular, steak or chicken?

    Answer: Chicken Burritos

    I had trouble with this first, because I wasn't taking into account case sensitivity.

    Step1: Count unique instances of "Steak Burrito" orders

    cut -f 3 chiptle.tsv | grep "Steak Burrito" | wc -1 
    or
    cat chipotle.tsv | cut -f 3 | grep "Steak Burrito" | wc -1

    returns: 368

    cut -f 3 chiptle.tsv | grep "Chicken Burrito" | wc -1
    or
    cat chipotle.tsv | cut -f 3 | grep "Chicken Burrito" | wc -1

    returns: 553


    *Question 5:* Do chicken burritos more often have black beans or pinto beans?

    Answer: Black Beans

    cut -f 3-3 chipotle.tsv | grep "Chicken Burrito" | grep "Black Beans" | wc -l

    Result: 282

     cut -f 3-3 chipotle.tsv | grep "Chicken Burrito" | grep "Pinto Beans" | wc -l

     Result: 105


    *Question 6:* Make a list of all of the CSV or TSV files in the DAT8 repo (using a single command). Think about how wildcard characters can help you with this task.

    The following command worked:

    precondition: My working directory was dat-dc-10

    find . -name "*.csv" -or -name "*.tsv"

    *also worked with single quotes

    *Question 7:*
    Count the approximate number of occurrences of the word "dictionary" (regardless of case) across all files in the DAT8 repo.

    Answer: 190

    grep -r dictionary* | wc -1

    *Question 8:*  Optional: Use the the command line to discover something "interesting" about the Chipotle data. Try using the commands from the "advanced" section!

    To get a better sense of the data, I executed the following:

    cut -f 1-4 chipotle.tsv

    To figure how many unique order variations, to include the one-to-many relationship between and item and it's description (ingredient variations):

    cut -f 3-4 chipotle.tsv | sort | uniq | wc -1

    To figure out how many order variations included guac:

    cut -f 3-4 chipotle.tsv | grep Guacamole* | sort | uniq | wc -l

    To discover which item descriptions returned a value of NULL, which could be concerning from an inventory accounting perspective for items that contain more than one itemizable item:

    cut -f 3-4 chipotle.tsv | grep NULL | sort unique

    Results indicated that Bottled Water and many "Chips and" variations were NULL. Adding wc -l showed that 12 items had NULL descriptions (11 Chips and; 1 Bottled Water)
