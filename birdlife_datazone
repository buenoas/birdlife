##########################
### BIRDLIFE DATA ZONE ###
### WEB SCRAPING #########
##########################
#options(warn=-1)

# Load required package
library(stringr)

# Import data
birdlife = read.csv("https://raw.githubusercontent.com/buenoas/birdlife/main/HBW_BirdLife_List_of_birds_v7.csv")

birdlife$Forest.dependency = NA
birdlife$Migratory.status = NA
birdlife$Eoo.breeding.km2 = NA
birdlife$Eoo.non_breeding.km2 = NA

# WEB SCRAPING
for (i in 1:nrow(birdlife)) {
  
  url = str_to_lower(
    gsub(" ", "-", 
         paste0("http://datazone.birdlife.org/species/factsheet/",
                paste(birdlife$Common.name[i],
                      birdlife$Scientific.name[i], sep = "-"),
                "/details")))
  url = gsub("'", "", url)
  url = gsub("ç", "c", url)
  url = gsub("ü", "u", url)
  
  sp = readLines(url)
  
  birdlife[i, 7] = str_squish(sp[which(grepl("Forest dependency", sp)) + 1])
  birdlife[i, 8] = str_squish(sp[which(grepl("Migratory status", sp)) + 1])
  birdlife[i, 9] = str_squish(sp[which(grepl("Extent of Occurrence \\(breeding/resident)", sp)) + 1])
  birdlife[i, 10] = ifelse(length(str_squish(sp[which(grepl("Extent of Occurrence \\(non-breeding)", sp)) + 1])) == 0, NA, str_squish(sp[which(grepl("Extent of Occurrence \\(non-breeding)", sp)) + 1]))
  
  print(paste0(i, " | ",
               birdlife$Scientific.name[i], " | ",
               round(i/nrow(birdlife) * 100, 2), "%"))
  
}


birdlife$Forest.dependency = gsub("<td>", "", birdlife$Forest.dependency)
birdlife$Forest.dependency = gsub("</td>", "", birdlife$Forest.dependency)

birdlife$Migratory.status = gsub("<td>", "", birdlife$Migratory.status)
birdlife$Migratory.status = gsub("</td>", "", birdlife$Migratory.status)

birdlife$Eoo.breeding.km2 = gsub("<td>", "", birdlife$Eoo.breeding.km2)
birdlife$Eoo.breeding.km2 = gsub(" km<sup>2</sup></td>", "", birdlife$Eoo.breeding.km2)
birdlife$Eoo.breeding.km2 = as.numeric(gsub(",", "", birdlife$Eoo.breeding.km2))

birdlife$Eoo.non_breeding.km2 = gsub("<td>", "", birdlife$Eoo.non_breeding.km2)
birdlife$Eoo.non_breeding.km2 = gsub(" km<sup>2</sup></td>", "", birdlife$Eoo.non_breeding.km2)
birdlife$Eoo.non_breeding.km2 = as.numeric(gsub(",", "", birdlife$Eoo.non_breeding.km2))


# See part of the results
rbind(head(birdlife),
      tail(birdlife))


# Save the results
write.csv(birdlife, "birdlife_datazone.csv", row.names = FALSE)
