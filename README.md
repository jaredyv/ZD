# Smartling-Zendesk-Script
Python Script for Synchronizing Zendesk articles with Smartling via API

<b>INSTALLATION</b> 

Backup current Zendesk HelpCenter content using existing scripts
Install prerequisite python libraries. Run terminal and enter commands below. (You’ll need to enter your Mac password.)
sudo easy_install lxml
sudo easy_install zdesk
Install script, including Smartling SDK
Set configuration values
Backup existing content using new script
Test articles:
Create test workflow and make it the default temporarily
Transfer 1 article from Zendesk to Smartling
Confirm that existing translations are picked up in Smartling (e.g., German should be 100%??)
Transfer 1 translated article from Smartling to Zendesk, 1 locale
Confirm that translated article works as expected in Zendesk
Repeat for a set of articles, 2 locales, many locales
Review log file
Test categories and sections
Transfer 1 of each to Smartling
Review log file
Transfer all categories and sections
Transfer all articles gradually, using exclusions initially, confirming at each step
Backup again
(Later) Remove old CSV files from Smartling project.

<b>NOTES</b>

Be aware of handling of ‘draft’ articles and incomplete translations in Smartling:

Transferring to Smartling:
‘all’ option: draft articles not included
Article IDs specified: draft articles are included

Transferring from Smartling:
‘all’ option: Only completed items included. Also, draft articles not included.
‘IDs specified: Incomplete items included (even if ‘published’ specified). Draft articles included.


Exclusions apply for both directions, so it could stop pending translations coming back. Therefore, don't start working on an item until its previous translations are live. Can workaround by pulling specific articles

If a particular item type (e.g., categories) is not specified then none of that type are transferred

Authorizes to all languages in Smartling. Alternative is to set approve ‘off’ and authorise in Smartling.

Previously transferred content is transferred again with the ‘all’ option. There’s no ‘all since’ logic. Therefore changes made in ZD could be overwritten. These items should be excluded.

Currently, all hyperlinks containing ‘/en-us/’ in the path are updated to point to the translated version instead. This may need to be refined depending on what sort of links are on the actual articles.

<b>COMMAND-LINE OPTIONS</b>

Option                      

Description
-t, --translate             

Transfer source content from ZenDesk to Smartling

-r, --retrievetranslations  

Transfer translations from Smartling to Zendesk

-l, --locales               

Comma-separated list of Zendesk locales or ‘all’. Applies to the ‘-r’ option.

-a, --articles              

Comma-separated list of Zendesk article IDs or ‘all’. 

-c, --categories            

Comma-separated list of Zendesk category IDs or ‘all’. 

-s, --sections              

Comma-separated list of Zendesk section IDs or ‘all’. 

-y, --retrievaltype         

What type of content to pull from Smarling: published, pending, or pseudo (see Smartling online help)

-g, --loglevel              

The least-critical level of log messages to include in the log file. Valid values: info, warning, error, critical, debug


<b>SAMPLE USAGE</b>

Transfer all completed translations from Smartling to Zendesk:

./smartlingzd.py --retrievetranslations --articles all --categories all --sections all -locales all

Transfer all articles from Zendesk to Smartling for translation:

./smartlingzd.py -t -a all -c all -s all -l all

Send two articles and all completed categories from Zendesk to Smartling, with detailed logging:

./smartlingzd.py --translate --articles 901922090,901922091 --categories all --loglevel debug
Or:
./smartlingzd.py -t -a 901922090,901922091 -c all -g debug

Tranfer the published French and German translations for the specified article from Smartling to Zendesk:

 ./smartlingzd.py --retrievetranslations --articles 901922090 --retrievaltype published –-locales fr,de
Or:
./smartlingzd.py -r -a 901922090 -y published -l fr,de

 Display help on command-line options :

./smartlingzd -h
    
    
<b>smartlingzd.cfg</b>

; Configuration file for the Smartling-Zendesk integration script.
; Contains basic parameters required for the script to work.

[general]
log_file = smartlingzd.log

[smartling]
api_key =
project_id = 
approve_for_translation = yes

[zendesk]
url = https://adroll.zendesk.com
user = 
auth_token =

[zd-to-sl-locales]
fr = fr-fr
de = de-de
nl = nl-nl
es = es-es
ja = ja


<b>translate.cfg</b>

; Optional configuration file for the Smartling-Zendesk integration script.
; Specifies which articles include/exclude in transfers from Zendesk to Smartling
; The file is used when the 'all' option is specified for articles on the command line. 

; item IDs should be listed under each section, with each ID on a separate line
; if nothing is included in a section, it means 'all' for 'include' sections and 'none' for 
; 'exclude' sections

[include-articles]
123456
234567


[exclude-articles]
987654


