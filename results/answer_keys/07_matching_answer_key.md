# Matching
## Exercise 1 Solution
1. Using the `A` and `B` vectors created above, evaluate each element in `B` to see if there is a match in `A`
	
	```r
	B %in% A
	```
2. Subset the `B` vector to only return those values that are also in `A`.
	
	```r
	intersectionBA <- B %in% A
	B[intersectionBA]
	
	# or you could do the two above steps in a single line of code:
	B[B %in% A]
	```

## Exercise 2 Solution

We have a list of 6 marker genes of that we are very interested in. Our goal is to extract count data for these genes, without having to scroll through the data frame of count data, using the `%in%` operator.

First, lets create a vector called `important_genes` with the Ensembl IDs of the 6 genes we are interested in:

```r
important_genes <- c("ENSMUSG00000083700", "ENSMUSG00000080990", "ENSMUSG00000065619", "ENSMUSG00000047945", "ENSMUSG00000081010", "ENSMUSG00000030970")
```

1. Use the `%in%` operator to determine if all of these genes are in the row names of the `rpkm_data` data frame.	

```r
important_genes %in% rownames(rpkm_data)
```
	
2. Extract the rows from `rpkm_data` that correspond to these 6 genes using `[]` and the `%in%` operator, again. Double check the row names to ensure that you are extracting the correct rows.

```r
intersection <- rownames(rpkm_data) %in% important_genes

rpkm_data[intersection, ]
```
  
3. **Bonus question:** Extract the rows from `rpkm_data` that correspond to these 6 genes using `[]`, but without using the `%in%` operator.

```r
rpkm_data[important_genes, ]
```

## Exercise 3 Solution
For a research project, we asked healthy volunteers and cancer patients questions about their diet and exercise. We also collected blood work for each individual, and each person was given a unique ID. Create the following dataframes, behavior and blood by copy/pasting the code below:

```r
# Creating behavior dataframe
ID <- c(546, 983, 042, 952, 853, 061)
diet <- c("veg", "pes", "omni", "omni", "omni", "omni")
exercise <- c("high", "low", "low", "low", "med", "high")
behavior <- data.frame(ID, diet, exercise)

# Creating blood dataframe

ID <- c(983, 952, 704, 555, 853, 061, 042, 237, 145, 581, 249, 467, 841, 546)
blood_levels <- c(43543, 465, 4634, 94568, 134, 347, 2345, 5439, 850, 6840, 5483, 66452, 54371, 1347)
blood <- data.frame(ID, blood_levels)
```
1. We would like to see if we have diet and exercise information for all of our blood samples. Using the ID information, determine whether all individuals with blood samples have associated behavioral information. Which individuals do not have behavioral information?

  ```r
  # One way to subset the FALSE values
  blood$ID[!(blood$ID %in% behavior$ID)]

  # Equally good way to subset FALSE values
  blood$ID[which(blood$ID %in% behavior$ID == F)]
  ```

2. The samples lacking behavioral information correspond to individuals who opted out of the study after having their blood drawn. Subset the blood data to keep only samples that have behavioral information and save the dataframe back to the blood variable.

  ```r
  blood <- blood[blood$ID %in% behavior$ID, ]
  all(blood$ID %in% behavior$ID)
  ```

3. We would like to combine the blood and behavior dataframes together, but first we need to make sure the data is in the same order.

  **a.** Take a look at each of the dataframes and manually identify the correct order for the blood dataframe such that it matches the order of IDs in the behavior dataframe.

    ```r
    behavior
    blood
    # order of `blood` IDs to match `behavior` IDs is: 6, 1, 5, 2, 3, 4
    ```

  **b.** Reorder the blood data to match the order of the IDs in the behavior dataframe and save the reordered blood dataframe as blood_reordered. Hint: you will need to have a vector of index values from a. to reorder. Once you have created blood_reordered you can use the all() function as a sanity check to make sure it was done correctly.

    ```r
    blood_reordered <- blood[c(6, 1, 5, 2, 3, 4), ]
    all(blood_reordered$ID == behavior$ID)
    ```
  **c.** Combine the dataframes blood_reordered and behavior using the data.frame() function and save this to a new dataframe called blood_behavior. Note: you will find that there are now two "ID" columns, this will help verify that you have reordered correctly.

    ```r
    blood_behavior <- data.frame(blood_reordered, behavior)
    blood_behavior
    ```
## Exercise 4 Solution
Similar to the previous exercise, perform the reordering of the `blood` data to match the order of the IDs in the `behavior` dataframe, but this time use the `match()` function. Save the reordered blood dataframe as `blood_reordered_match`. 

  ```r
  blood_reordered_idx <- match(behavior$ID, blood$ID)
  blood_reordered_idx
  blood_reordered_match <- blood[blood_reordered_idx, ]
  ```
