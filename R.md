# R Programming in Ubuntu 20.04
R is one of mandatory programming languages in bigdata. We need to install R Base, and RStudio. There are many blogs how to install R. 
I have difficulties to add new packages due to lack of libraries needed for R setup. The following is what I did on my setup.

## Install R
Open terminal then type:
>`sudo apt-get install dirmngr gnupg apt-transport-https ca-certificates software-properties-common`

Add CRAN repository to system sources list:
>`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9`

>`sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'`

Then install the R base
>`sudo apt-get install r-base`

Test it by typing `R` in terminal then press enter. You should see R console in the terminal.

##Install tidyverse Package
This is the basic package that is mandatory you must have in R. First, we need some required libraries. From terminal type:
>`sudo apt-get install -y libxml2-dev libcurl4-openssl-dev libssl-dev libv8-dev`

Then install tidyverse:
>`sudo R`

>`install.packages("tidyverse")`

That's all. Happy coding!
