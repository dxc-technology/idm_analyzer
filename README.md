# DXC IDM Analyzer

DXC created the IDM Analyzer as deployable package for SAP Identity Management to analyze system environments and identify migration complexity drivers. It uses information from the IDM database to obtain general information like number of objects, number of connected applications and processes. The main purpose of this solution is to give you a first effort indication for the migration of your IDM, by applying a factor method to provide a weighted score.

Analyzer is designed to respect the privacy of personal information stored in the IDM database. It does therefore not provide any reports on identity or authorization data of your Identity Governance solution.

## Installation
The DXC IDM Analyzer ships in two packages:

- Installer package *com.dxc.idm.analyzer.installer*
- Analyzer toolkit *com.dxc.idm.analyzer*
  
For the installation, proceed with importing the installer package first (*com.dxc.idm.analyzer.installer*). This package only includes two jobs:

> 01 Create Attributes<br>
> 02 Create IDM role for UI access

The first jobs adds attributes to the IDM schema, which will be used to display the questionnaire in the WebDynPro report. The second job adds an internal role to the IDM database, which is used to grant access to the WebDynPro report and includes the permission to access the reports tab. By default this role is created to be hidden from all users except those assigned to the standard admin role.If you don't use this built-in role, you will need to adjust this job accordingly.

**Note:** IDM utilizes both processes and jobs, if your default dispatcher is not marked to run jobs or processes, please assign an appropriate dispatcher.

***Role definition:***
```bash
ROLE:IDM:DXC_ANALYZER:UI_ACCESS
DISPLAYNAME = DXC_IDM_Analyzer UI Access
DESCRIPTION = This role grants access to the WebDynPro Interface of the DXC IDM Analyzer and includes access to the reporting tab of the IDM UI.
MXAC_ENTRY / MXAC_MEMBERS = 2
MX_OWNER = MX_ROLE:ADMIN
MXMEMBER_MX_PRIVILEGE = MX_PRIV:WD:TAB_REPORT
```

After running both jobs you can import the Analyzer toolkit (package: *com.dxc.idm.analyzer*). If you only want use the base-functionality
Some features of the DXC IDM Analyzer require additional configuration:

### Job report

The job report requires information from the database that are not accessible by the standard runtime dabase user (by default: mxmc_rt). To allow reading of additional database tables a separate connection string needs to be defined in the package constants of IDM Analyzer:

```bash
DXC_ANALYZER_IDM_DB_OPER_CON_STR
```

## Usage

Using IDM Analyzer is quite simple:
1. To use IDM Analyzer assign the previously created role for access control to your user.
2. Open the IDM Self Services Tab - you should find a new entry for IDM Analyzer. Click on it.
3. IDM Anaylyzer is by default "empty", to start the anaylsis complete the questionnaire section and hit the confirmation button *Trigger Report creation*.
4. After short time you will be able to obtain the downloable CSV-Reports under the Reporting Tab in the IDM UI.
5. Depending on your NetWeaver cache settings, the WebDynPro Report will be updated within 1 hour. 

## Reset

If you want to reset the WebDynPro Report, you can run the Job ```04 - Clear UI report``` in the primary package for DXC IDM Analyzer ```com.dxc.idm.analyzer```. Depending on your NetWeaver cache settings, the WebDynPro Report will be updated within 1 hour.

## Uninstall

To remove the IDM Migration Analyzer from your system start by removing the package ```com.dxc.idm.analyzer```. Now you can run the uninstall job ```ZZ - Uninstall``` from the package ```com.dxc.idm.analyzer```. This will remove the created MX_Reports, the created role for the User Interface.
As final step you will need to manually remove the attributes created by the installer from your schema, you can identify them based on their prefix ```DXC_ANALYZER```.

## Support

The solution is provided as is and without support. However, our consultants are happy to engage with you and provide guidance how to use DXC IDM Analyzer through our **IDM migration roadmap consulting package**. You can learn more about this at [dxc.com](https://dxc.com/us/en/offerings/security/digital-identity). You can raise requests through this [Request Form - SAP IDM Migration](https://connect.dxc.technology/SAPIDM-registration.html). 

# Terms and Conditions
 Copyright (C) 2024 DXC Technology

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see [https://www.gnu.org/licenses](https://www.gnu.org/licenses/).
