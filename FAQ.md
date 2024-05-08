# Allowing nulls
If a field used in your SELECT may be returned empty, it's important to indicate as such in the Schema definition. Use square brackets around the `type` and add null like this:

`"My_Field_Name": { "type": ["string", "null"] }` 

In this example, we are showing that Web_First_Name and Web_Last_Name will always have a value, but Web_Email might be empty.

```json
{
    "type": "object",
    "properties": {
        "Web_First_Name": {
            "type": "string"
        },
        "Web_Last_Name": {
            "type": "string"
        },
        "Web_Email": {
            "type": [
                "string",
                "null"
            ]
        }
        }
    }
}
```

# Parsing JSON
* When Parsing JSON objects and you know the payload contains only a single record, parse it using the `first()` function to avoid PowerAutomate behind helpful and forcing you to work inside for-each loops in your Flows.
* Remove [] from sample payloads so that PowerAutomate understands you're working with a single-object and not an array.


## Input Sanitization
* End-Users will ruin anything you let them touch. OK, just kidding. But you do need to always be thinking about sanitizing user-input, especially as it relates to special characters used in JSON like a `"` . A double-quote showing up in the middle of a field used to build an Adaptive Card can really wreak havoc.

```
replace(body('Parse_Form_Response')?['Response'],'"',decodeUriComponent('%27'))
```

There are fancier ways, such as [this approach](https://www.tachytelic.net/2020/10/remove-unwanted-characters-power-automate-flow/#h-create-a-select-action-to-remove-any-invalid-characters-from-the-string).

# Creating Nested Objects/Foreign Keys
* The API allows for one-shot creation of nested objects such as a new Contact, in a new Household, with a new Address. 
* See the [Help Center](https://help.acst.com/en/ministryplatform/developer-resources/developer-resources/rest-api/tables#creating-records-post-0) API documentation for more details.
* This sample payload will create all three:

## Payload
```json
[
{  
  Company: 0,
  First_Name: "Taylor",
  Last_Name: "Testperson",
  Nickname: "Tay",
  Display_Name: "Testperson, Tay",
  Email_Address: "taylor@fakemail.com",
  Contact_Status_ID: 1,
  Mobile_Phone: '555-867-5309',
  Household_ID:  {
      Household_Name: "Testperson",
      Address_ID: {
          Address_Line_1: "215 Vine Street",
          City: "Scranton",
          "State/Region": "PA",
          Postal_Code: 18503
      }
    }
  }
]
```

## Response Body

```json
[
  {
    "Contact_ID": 616501,
    "Company": false,
    "Company_Name": null,
    "Display_Name": "Testperson, Tay",
    "Prefix_ID": null,
    "First_Name": "Taylor",
    "Middle_Name": null,
    "Last_Name": "Testperson",
    "Suffix_ID": null,
    "Nickname": "Tay",
    "Date_of_Birth": null,
    "Gender_ID": null,
    "Marital_Status_ID": null,
    "Contact_Status_ID": 1,
    "Household_ID": 297049,
    "Household_Position_ID": null,
    "Date_of_Death": null,
    "Participant_Record": null,
    "Donor_Record": null,
    "Contact_Methods": null,
    "Email_Address": "taylor@fakemail.com",
    "Mobile_Phone": '574-867-5309',
    "Company_Phone": null,
    "Pager_Phone": null,
    "Fax_Phone": null,
    "Facebook_Account": null,
    "Twitter_Account": null,
    "Professional_Information": null,
    "Web_Page": null,
    "Industry_ID": null,
    "Occupation_ID": null,
    "Occupation_Name": null,
    "SSN/EIN": null,
    "Anniversary_Date": null,
    "Current_School": null,
    "HS_Graduation_Year": null,
    "Communication_Preferences": null,
    "Bulk_Email_Opt_Out": false,
    "Email_Unlisted": false,
    "Mobile_Phone_Unlisted": false,
    "Do_Not_Text": false,
    "Remove_From_Directory": false,
    "Other_Information": null,
    "User_Account": null,
    "ID_Card": null,
    "Contact_GUID": "485f1dfc-70fa-4328-9e14-a7e2b976a91f",
    "Contact_End_Date": null,
    "_Contact_Setup_Date": "2024-05-08T14:18:32.36",
    "Email_Verified": false,
    "Mobile_Phone_Verified": false,
    "Maiden_Name": null
  }
]
```

* Note that Address_ID isn't returned as it is one table away from the context of Contacts, but we can follow-up with a subsequent GET to Households using the returned Household_ID.
