
```{r}
#######################################
# Delete all practice trials
######################################
experimental_df<-data%>%
  tidyr::drop_na(trial_counter)

#############################
# Replace all cells with None by NA
##############################
experimental_df<- experimental_df%>% dplyr::na_if("None")
experimental_df<- experimental_df%>% dplyr::na_if("")

```

```{R}
# exclude participant number 37669 only 17% solved correct and only with insight, also bilangual with Turkish as mothertongue
# subject 39823 was by accident included but did not adhere to the instructions, she/he slept to little, sleep<6h

mixedef_correct_incorrect<-mixedef_correct_incorrect%>%
  filter(subject != "37669" & subject != "39823" & baseline_RMSSD > 0)
```

```{R}
####################################
# Place all values on one line per trial/solved word puzzle
experimental_df<- experimental_df %>%
  group_by(subject, trial_counter)%>%
  summarise_all(funs(first(na.omit(.))))

experimental_df$solution_type<-as.factor(experimental_df$solution_type)

summary(experimental_df)
```

```{r}
#################################
# Delete all word puzzles that were not solved
###################################
experimental_df<-experimental_df%>%
  tidyr::drop_na(answer)
sum(is.na(experimental_df$answer))
summary(experimental_df$ACC)
summary(experimental_df)
