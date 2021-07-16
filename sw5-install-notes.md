
# SW5 Install Notes - Jacques - _7/15/2021_

## First try
* first ran `npm run setup` without running `npm install` and quickly encountered an error:
![error: no npm install](https://github.com/jacquesdebar/misc-imgs/blob/main/error1.png?raw=true)

* ran `npm install`, got through the options menu fine, but failed partway through the setup because ImageMagick was missing
![error: no npm install](https://github.com/jacquesdebar/misc-imgs/blob/main/error2.png?raw=true)

* installed ImageMagick via `brew install pkg-config imagemagick`
* used pear to install imagick php extension via `pecl install imagick`, hit an error
* checked php version with `php -v`. found php to be installed at 7.4
* debugged brew stuff using the following commands
    * `ls -l /usr/local/opt/`
    * `which convert`
    * `ls -la /usr/local/bin/convert`
    * `convert -version`
    * `php -i | grep php.ini`
    * `cat /usr/local/etc/php/7.4/php.ini | grep magic`

* output from the final command showed that imagemagick / imagick were not properly attached to php:
![error: no npm install](https://github.com/jacquesdebar/misc-imgs/blob/main/nomagic.png?raw=true)


## Troubleshooting ImageMagick
* opened php configuration file via `code /usr/local/etc/php/7.4/php.ini`
* checked the file for imagick to be enabled in the extensions section 
    * none found:
    ![no extension="imagick.so"](https://github.com/jacquesdebar/misc-imgs/blob/main/phpi0.png?raw=true)

* added `extension="imagick.so"` in the extensions section, uncommented
![single extension="imagick.so"](https://github.com/jacquesdebar/misc-imgs/blob/main/phpi1.png?raw=true)

* ran `cat /usr/local/etc/php/7.4/php.ini | grep magic` again to check that imagick was working
    * got no output
    * this meant imagick still wasn't properly enabled

* used pear to install an older version of imagick via `pecl install imagick-3.4.4`
* checked php configuration file again
* two lines of `extension="imagick.so"` were in the extensions section:
    ![two of extension="imagick.so"](https://github.com/jacquesdebar/misc-imgs/blob/main/phpi2.png?raw=true)

    * `pecl install imagick-3.4.4` must have also added that lines
    * I manually removed one of the duplicate lines:
    ![single extension="imagick.so"](https://github.com/jacquesdebar/misc-imgs/blob/main/phpi1.png?raw=true)

* checked that imagick was installed again with `cat /usr/local/etc/php/7.4/php.ini | grep magic`
    * success!
    ![error: no npm install](https://github.com/jacquesdebar/misc-imgs/blob/main/magiccheck.png?raw=true)


## Second try
* ran `npm run setup` again, now that imagick was definitely installed
    * passed the `Check if ImageMagick exists` step now
    * hung up for 10+ minutes on `Cloning GitHub project [master]:
    ![stuck](https://github.com/jacquesdebar/misc-imgs/blob/main/stuck1.png?raw=true)

    * Hmmmmm...

* tried cloning single branch directly via `git clone --single-branch --branch master git@github.com:shortoftheweek/sotw5-web-app sw5`
    * terminal asks if I trust this repo/developer
    * I say yes
    * process continues
    * branch cloned successfully

* maybe the setup process was hung up because it didn't know if I trusted the repo/developer, but didn't have a line in the process to ask me about it?
    * re-ran `npm run setup`
    * passed the `Cloning  GitHub project [master]` step
    ![success](https://github.com/jacquesdebar/misc-imgs/blob/main/success1.png?raw=true)

    * other steps all pass successfully!


## Examining results in browser
* let's see if this works!
    * cd into directory
    * cd into sw5 sub-folder within directory: `cd sw5`
    * run `npm run serve`
    * navigate to http://127.0.0.1:8000/
        * seems to work, but showing 404 page...
        ![404 page](https://github.com/jacquesdebar/misc-imgs/blob/main/page404.png?raw=true)
    
    * refresh, same page shows
    * navigate to Explore page by clicking EXPLORE
    * looks good!
        * domain auto-switches to http://localhost:8000/explore instead of http://127.0.0.1:8000/explore
        ![explore page](https://github.com/jacquesdebar/misc-imgs/blob/main/explorepage.png?raw=true)
    
    * navigate back to homepage by clicking SOTW logo
    * looks good also!
        * domain at http://localhost:8000/
        * __success!!!__
        ![success](https://github.com/jacquesdebar/misc-imgs/blob/main/success2.png?raw=true)