####Robert Anderson
####Jan 2021
####Code for web scraping, data processing, and text mining which I used in my manufacturing relocations thesis



####/////Global variables/////
import os, requests, bs4, time, pandas, pprint, csv, openpyxl
headers = {'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36'}
time_delay = 3  #Time delay for web scraping, in seconds. This is used when I am scraping multiple urls and am worried about being blocked by a website.
destination_directory = r'C:\Users\Bob\Documents\My Research\Zheda thesis research\Firms in China\Output'   #General destination directory for output of web scraping and text mining

####/////General-use functions/////

#Export list with sublists to a csv as a single column
def export_superlist(results_superlist, destination_directory):
    outputfile = open(os.path.join(destination_directory,'output.csv'),'w', newline='', encoding='utf-8')
    outputwriter = csv.writer(outputfile)
    for i in range(0,len(results_superlist)):   #Iterates through sublist items and appends them to the end of the output column
        for j in range(0,len(results_superlist[i])):
            outputwriter.writerow([results_superlist[i][j]])
            #outputwriter.writerow([results_superlist[i][j].text])
    outputfile.close


##Export dataframe as CSV
def export_dataframe(dataframe, output_directory, output_filename):
    dataframe.to_csv(path_or_buf = os.path.join(output_directory, output_filename))
    print('\n'+output_filename + ' contains ' + str(len(dataframe)) + ' rows')


##Export set of lists as a CSV table with headers
def export_lists_to_table(lists, destination_directory, header_name_list):
    print()
    #TODO

####/////Case-specific functions (and case-specific global variables)/////

###//Flow control for functions//
#-------------MANUALLY CHANGE WHICH FUNCTIONS TO ACTIVATE-----------------
###This dictionary's keys are a list of all functions in this code. Change a value from 'N' to 'Y' to run the function; otherwise the function is skipped
activate_function = {
    'amcham_multiscrape': 'N',  #Note: please see this code to change the target URL
    'EDGAR_index_scrape': 'N',
    'EDGAR_index_aggregation': 'N',
    'EDGAR_index_drop_values': 'N',
    'EDGAR_index_key_files_export': 'N',
    'EDGAR_index_key_files_import': 'N',
    'EDGAR_index_companynames_CSV_export': 'N',
    'SEC_registration_scrape' : 'Y'    #Extracts business address and Standard Industrial Classification (SIC) from SEC registration page
    }
#---------------------------END----------------------------------------


###//Scrape: AmCham China Members//
def amcham_single_scrape(url, CSS_selector):
    #Getting HTML text from website
    req = requests.get(url, headers = headers)

    #Making sure website request was successful
    if req.status_code != 200:
        print('Error! Request denied on URL:')
        print(url)
    else:
        pass

    #Parsing HTML text
    amcham_soup = bs4.BeautifulSoup(req.text, features='html.parser')
    soup_results = amcham_soup.select(CSS_selector)    #This result is returned as a 'bs4.element.ResultSet' class

    #Convert result into a list of strings
    results_list = []
    for i in range(0, len(soup_results)):
        results_list.append(soup_results[i].text)

    #Returning results
    return results_list


def amcham_multi_scrape(time_delay):

    #----------MANUALLY CHANGE THIS PART TO SCRAPE DIFFERENT WEB.ARCHIVE SNAPSHOTS----------
    #(Please see accompanying Excel file for list of snapshots scraped
    #URLs to scrape
    amcham_url_staticblock =r'https://web.archive.org/web/20130211080703/http://www.amchamchina.org/directory/corporate?class=&firstletter='
    amcham_url_dynamicblock = [
        'A',
        'B',
        'C',
    ]
    '''
        'D',
        'E',
        'F',
        'G',
        'H',
        'I',
        'J',
        'K',
        'L',
        'M',
        'N',
        'O',
        'P',
        'Q',
        'R',
        'S',
        'T',
        'U',
        'V',
        'W',
        'X',
        'Y',
        'Z',
        '0'
        ]
    '''
    CSS_selector = 'dt a'


    #--------------------END--------------------------


    #The results list for each url will be appended to a superlist
    amcham_results_superlist = []
    for i in range(0,len(amcham_url_dynamicblock)):
        #Creating url to scrape from the static and dynamic url blocks
        amcham_url = amcham_url_staticblock + amcham_url_dynamicblock[i]
        #Scraping url and appending results to superlist
        amcham_results_superlist.append(amcham_single_scrape(amcham_url, CSS_selector))
        #Time delay to make sure the website doesn't block me
        time.sleep(time_delay)
    export_superlist(amcham_results_superlist, destination_directory)


#Activate scrape function
if activate_function['amcham_multiscrape'] == 'Y':
    amcham_multi_scrape(time_delay)


###//Scrape: EDGAR index files//

def EDGAR_index_scrape():

    #-----------MANUALLY CHANGE THIS PART TO SCRAPE DIFFERENT FILES------------
    #URL to scrape:
    #Example of full URL: https://www.sec.gov/Archives/edgar/full-index/2011/QTR1/
    EDGAR_url_staticblock1 =r'https://www.sec.gov/Archives/edgar/full-index/'
    EDGAR_url_dynamicblock1 = range(2010,2019 + 1)
    EDGAR_url_staticblock2 = r'/QTR'
    EDGAR_url_dynamicblock2 = [1,2,3,4]
    EDGAR_url_staticblock3 = r'/master.idx'

    #Destination directory
    destination_directory = r'C:\Users\Bob\Documents\My Research\Zheda thesis research\EDGAR\Output'
    #-------------------------------END---------------------------------------

    for year in EDGAR_url_dynamicblock1:
        for quarter in EDGAR_url_dynamicblock2:
            EDGAR_url = EDGAR_url_staticblock1 + str(year) + EDGAR_url_staticblock2 + str(quarter) + EDGAR_url_staticblock3

            #Getting file from website
            req = requests.get(EDGAR_url, headers = headers)

            #Making sure website request was successful
            if req.status_code != 200:
                print('Error! Request denied on URL:')
                print(url)
            else:
                #---------MANUALLY CHANGE OUTPUT FILENAME IF NEEDED------------
                output_filename = 'EDGAR master index ' + str(year) + 'Q' + str(quarter) + '.txt'
                #-----------------------END-----------------------------------
                outputfile = open(os.path.join(destination_directory, output_filename),'wb')
                
                outputfile.write(req.content)
                outputfile.close


#Activate scrape function
if activate_function['EDGAR_index_scrape'] == 'Y':
    EDGAR_index_scrape()



###//Using EDGAR index files//


##/Aggregating data/

#Directories
#------MANUALLY CHANGE DIRECTORY DESTINATION IF NEEDED-----------
EDGAR_indices_input_directory = r'C:\Users\Bob\Documents\My Research\Zheda thesis research\EDGAR\Index files\Raw'
EDGAR_indices_output_directory = r'C:\Users\Bob\Documents\My Research\Zheda thesis research\EDGAR\Index files\Aggregated'
#-------------------------END------------------------------------

#Parameters of files to read (this shouldn't need to be changed unless EDGAR changes their filing format)
EDGAR_index_delimiter = '|'
EDGAR_index_skiprow_list = list(range(0,9))+[10]


#Function
def aggregate_csv_files(input_directory, delimiter, skiprow_list):
    #Get list of files in test directory
    filelist = os.listdir(input_directory)

    #Read first dataset
    data = pandas.read_csv(os.path.join(input_directory,filelist[0]), sep = delimiter, engine = 'python', skiprows=skiprow_list)
    
    #Appending additional CSV files to initial file
    for filename in filelist[1:len(filelist)]: #Loops over all items in filelist except for the first item
        data_to_append = pandas.read_csv(os.path.join(input_directory,filename), sep = delimiter, engine = 'python', skiprows=skiprow_list)
        data = data.append(data_to_append, ignore_index=True)  #Ignore index causes the original index numbers of the appended rows to be overridden with index numbers matching the original file

    #Print row length for reference
    print('\nLength of aggregated dataframe: ' + str(len(data)) + ' rows')

    return data


#Activate aggregation function
if activate_function['EDGAR_index_aggregation'] == 'Y':
    EDGAR_index_data = aggregate_csv_files(EDGAR_indices_input_directory, EDGAR_index_delimiter, EDGAR_index_skiprow_list)



##/Dropping all form types except 10-Q, 10-K, 8-K, and variants/

#Terms to keep (these are effectively wildcard, i.e. *keepterm*
EDGAR_keeplist = [
    '10-Q',
    '10-K',
    '8-K'
    ]

EDGAR_hard_droplist = [     #These are terms which are to be dropped. They are otherwise picked up by the wildcarded keeplist
    '18-K',
    '18-K/A'
    ]


#Function
def drop_values_from_dataframe(dataframe, columnname , keeplist, hard_droplist):

    #Create blank droplist
    droplist = []   #Droplist is initially blank but has values added to it if they are not in the keeplist (the keepterms are effectively wildcarded, i.e. *keepterm*)

    #Add values to droplist
    for uniquevalue in dataframe[columnname].unique():
        if any(keepterm in uniquevalue for keepterm in keeplist) == False:
            droplist.append(uniquevalue)

    droplist.append(hard_droplist)  #Adding hard droplist terms

    #Drop rows from dataframe which have values in the droplist
    output_dataframe = dataframe[~dataframe[columnname].isin(droplist)]  # ~ reverses the result of dataframe.isin(droplist)

    #Reset index in dataframe
    output_dataframe = output_dataframe.reset_index()
    del output_dataframe['index']   #This step is necessary since the old index is preserved in a new column named 'index'

    #Return resulting dataframe
    return output_dataframe


#Activate dropping function
if activate_function['EDGAR_index_drop_values'] == 'Y':
    EDGAR_index_data_keyforms_trim = drop_values_from_dataframe(EDGAR_index_data, 'Form Type', EDGAR_keeplist, EDGAR_hard_droplist)

#Activate export function
EDGAR_keyfiles_export_filename = 'EDGAR_index_2010-2019_key_files.csv'  #Note: Do not comment out this line even if the data is already exported. This is used later to import the exported data.

if activate_function['EDGAR_index_key_files_export'] == 'Y':
    export_dataframe(EDGAR_index_data_keyforms_trim, EDGAR_indices_output_directory, EDGAR_keyfiles_export_filename)



##/Export list of company names/

#Activate function
if activate_function['EDGAR_index_companynames_CSV_export'] == 'Y':

    #Note: CIK does not map to Company Name at 1:1.

    #Import key files CSV (this step is not necessary for the code to run through, it is just to save time if EDGAR_index_data_keyforms_trim has already been exported and saved
    if activate_function['EDGAR_index_key_files_import'] == 'Y':
        EDGAR_index_data_keyforms_trim = pandas.read_csv(os.path.join(EDGAR_indices_output_directory, EDGAR_keyfiles_export_filename), engine = 'python', sep = ',')

    #Drop other columns besides Company Name
    EDGAR_index_unique_companies = EDGAR_index_data_keyforms_trim.drop(columns = ['Date Filed', 'Form Type', 'Filename', 'Unnamed: 0'])
    EDGAR_index_unique_companies = EDGAR_index_unique_companies.drop_duplicates()

    #Export
    export_dataframe(EDGAR_index_unique_companies, EDGAR_indices_output_directory, 'EDGAR_index_2010-2019_companylist.csv')
    print('Length of export: ' + str(len(EDGAR_index_unique_companies)))

    




###//Scrape: SEC registration on industry classification and business address of firms//

def SEC_registration_scrape(CIK_list):


    #CSS Selectors
    CSS_selector_address = 'td span.locality'

##    SEC_selectors : {
##    'City': 'td span.locality' ,
##    'ISIC': 'td:-soup-contains(ISIC)' ,
##    'NAICS': 'td:-soup-contains(NAICS)' ,
##    'SIC': 'td:-soup-contains(SIC)' 
##    }

    #Static URL block
    SEC_reg_URL_static_block = r'https://sec.report/CIK/'

    #Empty results list
    results_superlist = []

    for CIK in CIK_list:
        #Dynamic URL block
        SEC_reg_URL_dynamic_block = CIK
        #Complete URL
        SEC_reg_URL = os.path.join(SEC_reg_URL_static_block, SEC_reg_URL_dynamic_block)

        #Getting file from website
        req = requests.get(SEC_reg_URL, headers = headers)

        #Making sure website request was successful
        if req.status_code != 200:
            print('Error! Request denied on URL:')
            print(url)
        else:
            pass

        #Parsing HTML text
        SEC_reg_soup = bs4.BeautifulSoup(req.text, features='html.parser')
        #Using CSS selector:
        address_result = SEC_reg_soup.select(CSS_selector_address)[0].getText()
        #Using find text
        #   Explanation: Since this data is formatted as a cell entry in a table and doesn't have a named class,
        #   I use soupname.find(elementname, text = "searchtext") to search the cells for specific text.
        #   The text I search for gives the cell before the cell of interest, so I use .next_sibling to give
        #   the cell of interest. Finally, .text is used to give the content of the HTML tag (i.e. strip out the HTML
        #   formatting from this element)
        NAICS_result = SEC_reg_soup.find("td", text = "NAICS").next_sibling.text
        ISIC_result = SEC_reg_soup.find("td", text = "ISIC").next_sibling.text
        SIC_result = SEC_reg_soup.find("td", text = "SIC").next_sibling.text


        #Append result
        results_superlist.append([
            CIK,
            address_result,
            NAICS_result,
            ISIC_result,
            SIC_result
            ])

        #Time delay for responsible scraping
        time.sleep(time_delay)

    return results_superlist

#Code for running test
CIK_list = ['0001138723','0001734107', '0000320193']


#Activate function
if activate_function['SEC_registration_scrape'] == 'Y':

    #Import input data
    #----Note: As currently program, the input data should not have a header or have duplicates-----
    input_directory = r'C:\Users\Bob\Documents\My Research\Zheda thesis research\Firms in China\Output\Input'
    input_filename = r'CIK list.xlsx'
    # ----- End note -----
    CIKs = pandas.read_excel(os.path.join(input_directory, input_filename),
                             sheet_name = 'Sheet1',
                             header = None,
                             engine = 'openpyxl', #Note: the default engine xlrd only supports old-style Excel files (.xls). The openpyxl engine is needed for .xlsx Excel files.
                             index_col = None,
                             converters = {0 : '{:0>10}'.format}    #Adds leading zeros up to 10 digits, which is what CIKs have.
                             )
    
##    #Scrape function
##    SEC_reg_scrape_results = SEC_registration_scrape(CIK_list)
##
##    #Export result
##    export_superlist(SEC_reg_scrape_results, destination_directory)
    



#Todo:

#   *COde import
#   *Code export


##
###Filter on unique company names
##EDGAR_index_unique_companies = EDGAR_index_data_keyforms_trim['Company Name'].unique()
##
###Export
##if activate_function['EDGAR_index_companynames_CSV_export'] == 'Y':
##    numpy.savetxt(os.path.join(EDGAR_indices_output_directory, 'EDGAR_index_2010-2019_companylist.csv'), EDGAR_index_unique_companies, delimiter = ',', encoding = 'utf-8')
##    #export_dataframe(EDGAR_index_unique_companies, EDGAR_indices_output_directory, 'EDGAR_index_2010-2019_companylist.csv')





####Code for future reference:

##Iterating over a range of the entire alphabet (lower case only)
#for i in range(ord('A'),ord('Z')+1):
#	print(chr(i))


##Reading bs4 results

#amcham_results[0].text

#for i in range(0,len(amcham_results)):
#    print(amcham_results[i].text)
