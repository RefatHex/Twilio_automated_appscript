# WhatsApp Survey System - Comprehensive User Guide

## Table of Contents

1. [System Overview](#system-overview)
2. [Initial Setup](#initial-setup)
3. [Sheet Structure](#sheet-structure)
4. [Sending Surveys](#sending-surveys)
5. [Survey Templates](#survey-templates)
6. [Response Handling](#response-handling)
7. [Scheduling Surveys](#scheduling-surveys)
8. [Region and Language Support](#region-and-language-support)
9. [Troubleshooting](#troubleshooting)
10. [Advanced Features](#advanced-features)

## System Overview

This WhatsApp Survey System allows you to send automated customer satisfaction surveys via WhatsApp across multiple markets (UAE, KSA, Egypt, Kuwait) in appropriate languages (English/Arabic). The system supports various survey types (Installation, Training, Menu Upload, Shipping, Ad-Hoc) and includes features for scheduling, response tracking, and template management.

### Key Features

- **Multi-region support**: Send surveys from country-specific numbers
- **Language adaptation**: Automatically use English or Arabic based on region
- **Multiple survey templates**: Different templates for each phase of onboarding
- **Self-service template management**: Edit templates without coding
- **Ad-hoc survey capability**: Send custom surveys to selected recipients
- **Response tracking**: Capture detailed feedback including sub-reasons
- **Scheduling**: Plan surveys for future delivery
- **Follow-up messages**: Automated follow-up for negative responses

## Initial Setup

### Step 1: Twilio Configuration

1. Log in to your Twilio account
2. Obtain your Account SID and Auth Token from the dashboard
3. Create WhatsApp Messaging Services for each region (KSA, UAE, Egypt, Kuwait)
4. Create WhatsApp Templates for each survey type and language
5. Get the SIDs for all your messaging services and templates

### Step 2: Script Configuration

1. Open the Google Sheets document
2. Go to Extensions > Apps Script
3. Update the CONFIG object at the top of the script with your Twilio credentials:
   - Replace the `ACCOUNT_SID` and `AUTH_TOKEN` with your Twilio credentials
   - Update the `NUMBERS` object with your Messaging Service SIDs for each region
   - Update the `TEMPLATES` object with your Template SIDs for each survey type, language, and message type

### Step 3: Deploy as Web App

1. In the Apps Script editor, click on Deploy > New deployment
2. Select "Web app" as the deployment type
3. Set "Execute as" to "Me" (your Google account)
4. Set "Who has access" to "Anyone, even anonymous"
5. Click "Deploy" and authorize the necessary permissions
6. Copy the web app URL provided after deployment

### Step 4: Set Up Twilio Webhook

1. Go to your Twilio console
2. Navigate to Programmable Messaging > Settings > WhatsApp Sandbox
3. Paste the web app URL from Step 3 into the "WHEN A MESSAGE COMES IN" field
4. Save the configuration

### Step 5: Initialize Sheets

1. Return to your Google Sheet
2. Use the custom menu "Survey System" > "Setup Sheets" to create all necessary sheets with proper headers

## Sheet Structure

### TriggerSheet

This sheet is where you add phone numbers to send surveys.

| Column                        | Description                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------- |
| Phone Number                  | Recipient's phone number without the whatsapp: prefix or + (e.g., 966501234567) |
| Region (KSA/UAE/EGYPT/KUWAIT) | Recipient's region code                                                         |
| Survey Type                   | Type of survey to send (INSTALLATION, TRAINING, MENU_UPLOAD, SHIPPING, AD_HOC)  |
| Scheduled Time                | Optional: When to send the survey (leave blank for immediate sending)           |
| Survey Sent                   | Automatically filled (✅ or ❌)                                                 |
| Timestamp                     | Automatically filled with the time the survey was sent                          |
| Status                        | Automatically filled with the current status                                    |

### ResponseSheet

This sheet records all customer responses.

| Column           | Description                                   |
| ---------------- | --------------------------------------------- |
| Phone Number     | Respondent's phone number                     |
| Survey Type      | Type of survey that was sent                  |
| Language         | Language used (ENGLISH or ARABIC)             |
| Response         | Customer's response (Yes or No)               |
| Follow-up Reason | Reason provided if the customer selected "No" |
| Region           | Customer's region                             |
| Timestamp        | Time of response                              |
| Status           | Status of the response process                |

### TemplatesSheet

This sheet manages all survey templates.

| Column        | Description                                    |
| ------------- | ---------------------------------------------- |
| Survey Type   | Type of survey (e.g., INSTALLATION)            |
| Language      | Language of the template (ENGLISH or ARABIC)   |
| Message Type  | Type of message (SURVEY, FOLLOW_UP, THANK_YOU) |
| Template SID  | Twilio Template SID                            |
| Template Name | A descriptive name for the template            |
| Description   | Detailed description of the template           |

### WebhookLog

This sheet logs all incoming webhook data for debugging.

| Column         | Description                        |
| -------------- | ---------------------------------- |
| Timestamp      | Time of the webhook call           |
| Raw Data       | Raw JSON data received from Twilio |
| Processed Data | Extracted and processed data       |
| Status         | Processing status of the webhook   |

## Sending Surveys

### Method 1: Add to TriggerSheet

Simply add a row to the TriggerSheet with the phone number and region. The survey will be sent automatically.

### Method 2: Use the Ad-Hoc Survey Dialog

1. Click "Survey System" > "Send Ad-Hoc Survey"
2. Enter one or more phone numbers (one per line)
3. Select the region
4. Choose the survey type
5. Optionally set a scheduled time
6. Click "Send Survey"

### Method 3: Send a Test Message

1. Click "Survey System" > "Send Test Message"
2. Enter a single phone number
3. Select the region
4. Choose the survey type
5. Click "Send Test Survey"

## Survey Templates

### Template Structure

Each survey consists of three message types:

1. **SURVEY**: Initial survey question (CSAT Yes/No question)
2. **FOLLOW_UP**: Follow-up question asking for reasons if customer answers "No"
3. **THANK_YOU**: Thank you message sent after receiving a response

Each template is available in English and Arabic, depending on the region.

### Managing Templates

You can manage templates in two ways:

#### Method 1: Use the TemplatesSheet

1. Add or edit rows in the TemplatesSheet
2. Ensure you provide all required columns
3. Click "Survey System" > "Reload Templates" to apply changes

#### Method 2: Use Template Management Dialog

1. Click "Survey System" > "Manage Survey Types"
2. View existing survey types
3. Add new survey types as needed

### Creating New Templates in Twilio

1. Log in to your Twilio console
2. Navigate to Messaging > Content Editor
3. Create new templates for each survey type, language, and message type
4. For survey questions, use a Yes/No button template
5. For follow-up questions, use a text template
6. For thank you messages, use a text template
7. Add the Template SIDs to your TemplatesSheet

## Response Handling

The system automatically handles responses as follows:

### "Yes" Responses

- Records the response as "Yes" in the ResponseSheet
- Sends a thank you message
- Updates the status in the TriggerSheet

### "No" Responses

- Records the response as "No" in the ResponseSheet
- Sends a follow-up message asking for reasons
- Updates the status in the TriggerSheet

### Follow-up Responses

- Records the text as the "Follow-up Reason" in the ResponseSheet
- Sends a thank you message
- Updates the status in the TriggerSheet

## Scheduling Surveys

The system allows you to schedule surveys to be sent at a specific date and time in the future, which is useful for planning campaigns or sending surveys at optimal times.

### Method 1: Using the TriggerSheet

1. Add a row to the TriggerSheet
2. Enter the phone number, region, and survey type
3. In the "Scheduled Time" column, enter the future date and time
4. The system will check every 5 minutes and send surveys when it's time

#### Example:

| Phone Number | Region | Survey Type  | Scheduled Time      | Survey Sent | Timestamp | Status                                 |
| ------------ | ------ | ------------ | ------------------- | ----------- | --------- | -------------------------------------- |
| 966501234567 | KSA    | INSTALLATION | 2023-09-25 10:00:00 |             |           | Scheduled for Mon Sep 25 2023 10:00:00 |

When the scheduled time arrives, the system will automatically send the survey and update the row:

| Phone Number | Region | Survey Type  | Scheduled Time      | Survey Sent | Timestamp           | Status      |
| ------------ | ------ | ------------ | ------------------- | ----------- | ------------------- | ----------- |
| 966501234567 | KSA    | INSTALLATION | 2023-09-25 10:00:00 | ✅          | 2023-09-25 10:00:05 | Survey Sent |

### Method 2: Using the Ad-Hoc Survey Dialog

1. Open the Ad-Hoc Survey Dialog from the "Survey System" menu
2. Fill in all required fields:
   - Phone numbers (one per line)
   - Region
   - Survey type
3. Use the datetime picker to set a future time
4. Click "Send Survey"

#### Input Example:

![Ad-Hoc Survey Dialog Example](https://i.imgur.com/example1.png)
_Example shows scheduling multiple surveys for Sep 25, 2023 at 10:00 AM_

#### Output Example:

After clicking "Send Survey", you'll see a confirmation message:

```
Processed 3 numbers. Success: 3, Errors: 0. Surveys scheduled for 9/25/2023, 10:00:00 AM
```

And the TriggerSheet will show:

| Phone Number | Region | Survey Type  | Scheduled Time      | Survey Sent | Timestamp | Status    |
| ------------ | ------ | ------------ | ------------------- | ----------- | --------- | --------- |
| 966501234567 | KSA    | INSTALLATION | 2023-09-25 10:00:00 |             |           | Scheduled |
| 966512345678 | KSA    | INSTALLATION | 2023-09-25 10:00:00 |             |           | Scheduled |
| 966523456789 | KSA    | INSTALLATION | 2023-09-25 10:00:00 |             |           | Scheduled |

### Time Format

When entering a scheduled time in the TriggerSheet, use one of these formats:

1. **Google Sheets Date/Time**: Simply enter a date and time using the Google Sheets date picker
2. **ISO Format**: `YYYY-MM-DD HH:MM:SS` (e.g., `2023-09-25 10:00:00`)
3. **Local Format**: Your computer's local date/time format will also work

### Checking Scheduled Surveys

The TriggerSheet shows all scheduled surveys with:

- A future date in the "Scheduled Time" column
- An empty "Survey Sent" column
- A status of "Scheduled" or "Scheduled for [date/time]"

### Modifying or Canceling Scheduled Surveys

To cancel a scheduled survey:

1. Find the row in the TriggerSheet
2. Delete the row or clear the "Scheduled Time" cell

To reschedule a survey:

1. Find the row in the TriggerSheet
2. Change the date/time in the "Scheduled Time" cell

### How Scheduling Works

The system creates a time-based trigger that runs every 5 minutes. This trigger:

1. Checks the TriggerSheet for rows with:
   - A scheduled time that has passed
   - No value in the "Survey Sent" column
2. Sends the survey for each qualifying row
3. Updates the row with the sent status and timestamp

### Troubleshooting Scheduled Surveys

If scheduled surveys aren't being sent:

1. Check that the scheduled time has actually passed
2. Verify that the "Setup Scheduled Surveys" function has been run (via the menu)
3. Check the Apps Script dashboard for any errors in the time-based trigger
4. If needed, manually run "Survey System" > "Setup Scheduled Surveys" to recreate the trigger

## Region and Language Support

### Supported Regions

- **UAE**: Messages sent from UAE number in English
- **KSA**: Messages sent from KSA number in Arabic
- **Egypt**: Messages sent from Egypt number in Arabic
- **Kuwait**: Messages sent from Kuwait number in Arabic

### Language Mapping

The system automatically selects the appropriate language based on the region:

- UAE → English
- KSA, Egypt, Kuwait → Arabic

## Troubleshooting

### Common Issues and Solutions

#### Surveys Not Sending

1. Check your Twilio account balance
2. Verify that the phone numbers are in the correct format (no + or whatsapp: prefix)
3. Ensure the Twilio credentials are correct
4. Check the WebhookLog sheet for error messages

#### Responses Not Being Recorded

1. Verify that the webhook URL is correctly set in Twilio
2. Check that the Twilio WhatsApp Sandbox is properly configured
3. Examine the WebhookLog sheet for incoming messages
4. Ensure the deployed web app has the necessary permissions

#### Template Issues

1. Verify that all Template SIDs in the CONFIG object are correct
2. Ensure templates are approved in your Twilio account
3. Check that templates have the correct format (Yes/No buttons for surveys)
4. Use the "Debug Template" feature to test specific templates

### Debug Tools

#### Webhook Log

The WebhookLog sheet records all incoming webhook data, helping diagnose issues with responses.

#### Debug Template

1. Click "Survey System" > "Debug Template"
2. Enter a phone number, region, survey type, and message type
3. Click "Send Test Message" to test a specific template

## Advanced Features

### Adding New Survey Types

1. Click "Survey System" > "Manage Survey Types"
2. Enter the new survey type name (use UPPERCASE with underscores)
3. Click "Add Survey Type"
4. The system will create placeholder templates in the TemplatesSheet
5. Update the Template SIDs with actual values from Twilio

### Customizing Templates

For each survey type, you need templates for:

- Initial survey in English and Arabic
- Follow-up question in English and Arabic
- Thank you message in English and Arabic

Update the TemplatesSheet with the correct Template SIDs for each.

### Time-Based Triggers

The system automatically creates a time-based trigger that runs every 5 minutes to check for scheduled surveys. If you need to manually recreate this trigger:

1. Click "Survey System" > "Setup Scheduled Surveys"

## Technical Details

### Script Structure

- **CONFIG object**: Contains all configuration including Twilio credentials, template SIDs, and region settings
- **Helper functions**: Functions like `getLanguageForRegion()` and `getTemplateSid()` handle language selection and template retrieval
- **Send functions**: `sendSurvey()`, `sendFollowUp()`, and `sendThankYou()` handle message sending
- **Webhook handler**: `doPost()` processes incoming messages
- **Sheet management**: Functions to manage the various sheets and their data
- **UI handlers**: Functions that create and manage the user interface dialogs

### Webhook Processing Flow

1. Customer receives a survey and responds
2. Twilio sends the response to your webhook URL
3. The `doPost()` function processes the response
4. The system determines the appropriate action based on the response
5. The response is recorded in the ResponseSheet
6. A follow-up or thank you message is sent as appropriate

### Data Security

- Twilio credentials are stored in the script and not exposed in the sheets
- The webhook URL requires no authentication as it's designed to receive Twilio callbacks
- The script runs with the permissions of the deployer (you)
- No customer data is stored outside of your Google Sheet

## Contact and Support

If you encounter any issues or have questions about the system, please contact:

- Email: [your_email@example.com]
- Phone: [your_phone_number]

For Twilio-specific issues, refer to Twilio's documentation at https://www.twilio.com/docs

---

## Quick Reference Guide

### Survey System Menu Options

- **Setup Sheets**: Create or reset necessary sheets
- **Send Test Message**: Send a single test survey
- **Debug Template**: Test specific templates
- **Setup Scheduled Surveys**: Set up time-based triggers
- **Send Ad-Hoc Survey**: Send surveys to multiple recipients
- **Reload Templates**: Refresh templates from the TemplatesSheet
- **Manage Survey Types**: Add or view survey types

### Important Spreadsheet Tabs

- **TriggerSheet**: Where you add numbers to send surveys
- **ResponseSheet**: Where responses are recorded
- **TemplatesSheet**: Where you manage templates
- **WebhookLog**: Where webhook data is logged
- **Instructions**: This instruction document

### Template Types

- **SURVEY**: Initial CSAT question
- **FOLLOW_UP**: Question sent after "No" response
- **THANK_YOU**: Message sent after receiving response

### Survey Types

- **INSTALLATION**: Installation phase survey
- **TRAINING**: Training phase survey
- **MENU_UPLOAD**: Menu upload phase survey
- **SHIPPING**: Shipping phase survey
- **AD_HOC**: Custom ad-hoc surveys

### Schedule Formats

- **ISO Date**: YYYY-MM-DD HH:MM:SS (e.g., 2023-09-25 10:00:00)
- **Relative Time**: Not directly supported, but you can use Google Sheets formulas:
  - Tomorrow at 9am: `=TODAY()+1+TIME(9,0,0)`
  - Next week same time: `=TODAY()+7+TIME(HOUR(NOW()),MINUTE(NOW()),0)`
