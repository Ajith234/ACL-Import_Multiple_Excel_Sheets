ACL-Import_Multiple_Excel_Sheets
================================

ACL

Import all Excel Sheets
Objective – to allow the user to select an Excel workbook by navigating to the desired folder/directory and selecting the Excel file using a familiar tool, Windows File Explorer. All of the sheets within that workbook are then imported to separate tables and .FILs

Benefit – the user does not have to know the names of the individual sheets and execute mutple ACL IMPORT EXCEL commands. This also is useful if the sheet names are not predictable (i.e. are not - 01,02,031, or  Jan, Feb, Mar) or sheet names change from cycle to cycle.

Caution – This uses the ACL 9.1 and prior IMPORT EXCEL command structure where individual fields are NOT allowed to be selected or configured.  All columns in a sheet must be imported.  Also all sheets will be imported, sheets cannot be selected, only the source workbook may be selected.

Requirements – Powershell 2 or above must be installed on the machine running ACL Analytics 10. The Powershell script (Choose_a_Workbook_and_list_all_sheets.ps1) must be in a directory accessible by the user. That directory is specified in the main script with the variable v_powershellpath.  The Excel  workbook to be imported is assumed to be in the project path specified in the main script with the variable v_projectpath. 
ANALYTICS 10 EXECUTE Command --        EXECUTE "powershell.exe %v_powershellpath%Choose_a_Workbook_and_list_all_sheets.ps1 %v_powershellpath%" using variable substitution the source Excel file and the PS1 Powershell script can be anywhere, those 2 paths must be specified as variables above. One set of double quotes works well. The default SYNCH setting for the EXECUTE command is appropriate because we want Powershell to build the file name taxt file and the sheet list test file before any further processing. The second %v_powershellpath% string is a parameter passed to the Powershell to determine the location of where the write the 2 text output files.
Source File – The source Excel workbook ABC_Company_Combined_Trial_Balances.xlsx can be either an xls or an xlsx. The thee sheets inside the workbook are TB_12_31_2013, TB_12_31_2012, TB_12_31_2011. If these names were known this could be scripted in ACL, but what if the sheet name changes? The script will need to be changed for the new names. With the Powershell approach the three sheets will be brought in even if their names changes without having to alter the import script. (careful, downstream scripts expecting a certain table name my no longer work if the sheet names changes despite the successful import)
Powershell Note – The option –encoding ACSII is required for non-unicode ACL to build text files that can be read with an IMPORT DELIMITED command. (Powershell default is to create a UNICODE text file)


Processing – The PAUSE command up front informs the user that there is not column selection nor all sheet selection, everything in the workbook will be imported. Take this PAUSE command out for headless running. The file and path text files and the sheet list text file are IMPORT DELIMITED using a pipe delimiter, since there never is a pipe in the content the whole line is imported. The sheetlist records are counted which determines the maximum number of cycles through the subscript that performs the IMPORT EXCEL for each sheet. The ACL table name is a combination of up to 15 characters of the workbook and and up to 15 characters of the sheet name, some truncation will occur for longer workbook and/or longer sheet names. All sheets are imported, there is not pick and choose of sheets.
 

