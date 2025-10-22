# CSV Import Guide for Library Management System

## Overview
This guide explains how to import books and students using CSV files. Both administrators and librarians can perform CSV imports.

---

## Book Import

### CSV Format
The book import CSV file must have exactly these columns in this order:

```
ISBN,Book Name,Author,Date Published,Category,Pieces,Description
```

### Important Notes
- **DO NOT include a book_cover column** - Book covers cannot be imported via CSV
- Book covers must be added individually by editing each book after import
- All fields are required except Description (can be empty)
- ISBN must be unique - duplicate ISBNs will be rejected

### Sample Book CSV File
```csv
ISBN,Book Name,Author,Date Published,Category,Pieces,Description
978-0-134-68599-1,Effective Java,Joshua Bloch,2018,Programming,3,A comprehensive guide to Java programming best practices
978-0-596-51774-8,Programming Python,Mark Lutz,2010,Programming,2,Powerful object-oriented programming
978-1-491-95039-8,Fluent Python,Luciano Ramalho,2015,Programming,5,Clear concise and effective programming
978-0-13-468599-2,Clean Code,Robert Martin,2008,Programming,4,A handbook of agile software craftsmanship
978-1-59327-928-8,Python Crash Course,Eric Matthes,2019,Programming,6,A hands-on project-based introduction
```

### Field Descriptions

| Field | Required | Type | Description | Example |
|-------|----------|------|-------------|---------|
| ISBN | Yes | Text | International Standard Book Number (unique identifier) | 978-0-134-68599-1 |
| Book Name | Yes | Text | Title of the book | Effective Java |
| Author | Yes | Text | Author name(s) | Joshua Bloch |
| Date Published | No | Number | Year published (4 digits) | 2018 |
| Category | Yes | Text | Book category/genre | Programming |
| Pieces | No | Number | Number of copies (default: 1) | 3 |
| Description | No | Text | Book description/summary | A comprehensive guide... |

### Validation Rules
- **ISBN**: Must be unique (not already in database)
- **Date Published**: Must be 4-digit year (1000-9999) if provided
- **Pieces**: Must be positive integer (≥ 1)
- **Duplicate check**: Books with same ISBN will show error, not imported

### Import Steps
1. Prepare your CSV file following the format above
2. Save as UTF-8 encoded CSV file
3. Log in as Admin or Librarian
4. Navigate to Dashboard → Import Books
5. Upload your CSV file
6. Review import results
7. Fix any errors shown and re-import rejected rows

### Common Errors

| Error Message | Cause | Solution |
|--------------|-------|----------|
| "Missing ISBN" | ISBN column is empty | Fill in ISBN for all books |
| "Missing Book Name" | Book Name column is empty | Fill in title for all books |
| "Book with ISBN XXX already exists" | Duplicate ISBN | Check if book already exists or use different ISBN |
| "Invalid Date Published" | Year is not 4 digits | Use format: 2018 (not 18 or 2018-01-01) |
| "Invalid Pieces" | Not a number or ≤ 0 | Use positive integers only (1, 2, 3...) |
| "no such column: library_book.book_cover" | Old error (now fixed) | Update your system, this is fixed |

---

## Student Import

### CSV Format
The student import CSV file must have exactly these columns in this order:

```
Student ID,Last Name,First Name,Middle Name,Course,Year,Section
```

### Important Notes
- Middle Name can be left empty
- Students imported via CSV do not have user accounts initially
- Students must register themselves using their Student ID to create accounts
- All fields except Middle Name are required

### Sample Student CSV File
```csv
Student ID,Last Name,First Name,Middle Name,Course,Year,Section
2024-00001,Dela Cruz,Juan,Santos,BSIT,1,A
2024-00002,Garcia,Maria,Lopez,BSCS,2,B
2024-00003,Reyes,Pedro,,BSIS,3,C
2024-00004,Santos,Ana,Marie,BSIT,4,A
2024-00005,Ramos,Carlos,Domingo,BSCS,1,B
```

### Field Descriptions

| Field | Required | Type | Description | Example |
|-------|----------|------|-------------|---------|
| Student ID | Yes | Text | Unique student identifier | 2024-00001 |
| Last Name | Yes | Text | Student's last name/surname | Dela Cruz |
| First Name | Yes | Text | Student's given name | Juan |
| Middle Name | No | Text | Student's middle name (can be empty) | Santos |
| Course | Yes | Text | Degree program | BSIT |
| Year | Yes | Text | Year level (1, 2, 3, 4) | 1 |
| Section | Yes | Text | Class section | A |

### Validation Rules
- **Student ID**: Must be unique (not already in database)
- **All required fields**: Must have values (no empty cells)
- **Duplicate check**: Students with same Student ID will show error

### Import Steps
1. Prepare your CSV file following the format above
2. Save as UTF-8 encoded CSV file
3. Log in as Admin or Librarian
4. Navigate to Dashboard → Import Students
5. Upload your CSV file
6. Review import results
7. Fix any errors shown and re-import rejected rows

### Common Errors

| Error Message | Cause | Solution |
|--------------|-------|----------|
| "Missing Student ID" | Student ID column is empty | Fill in Student ID for all students |
| "Missing Last Name" | Last Name column is empty | Fill in last name for all students |
| "Missing First Name" | First Name column is empty | Fill in first name for all students |
| "Missing Course" | Course column is empty | Fill in course for all students |
| "Missing Year" | Year column is empty | Fill in year for all students |
| "Missing Section" | Section column is empty | Fill in section for all students |
| "Student with ID XXX already exists" | Duplicate Student ID | Check if student already exists or use different ID |

---

## CSV File Preparation Tips

### Excel Users
1. Create your spreadsheet in Excel
2. Use the exact column headers shown above
3. Fill in all data
4. Save As → CSV (Comma delimited) (*.csv)
5. Choose UTF-8 encoding if prompted

### Google Sheets Users
1. Create your spreadsheet in Google Sheets
2. Use the exact column headers shown above
3. Fill in all data
4. File → Download → Comma Separated Values (.csv)

### Encoding Issues
- Always save CSV files as **UTF-8** encoding
- This ensures special characters display correctly
- If you see weird characters after import, re-save as UTF-8

### Testing Your Import
1. Start with a small test file (2-3 rows)
2. Import the test file
3. Check for errors
4. If successful, import your full file
5. This saves time if there are format issues

---

## After Import

### Books
- Books are immediately added to the system
- Copies_available is set equal to Pieces
- Book covers are NULL (must be added individually)
- Books can be edited to add covers later

### Students
- Students are added to the database
- They do NOT have user accounts yet
- Students must self-register using their Student ID
- Admin must approve student registrations
- Students can then log in and use the system

---

## Download Sample Templates

Sample CSV files are included in the project:
- `sample_books.csv` - Example book import file
- `sample_students.csv` - Example student import file

Use these as templates for your own imports.

---

## Troubleshooting

### Import shows "0 books successfully imported"
- Check CSV format matches exactly (column names and order)
- Verify file is saved as CSV, not Excel (.xlsx)
- Check for encoding issues (save as UTF-8)
- Review error messages shown on screen

### Some rows imported, others failed
- This is normal! The system imports valid rows and reports errors for invalid ones
- Review the error messages to see which rows failed and why
- Fix the errors in those rows and import again
- Already imported rows will show "already exists" error (safe to ignore)

### "no such column: library_book.book_cover" error
- This error has been fixed in the latest version
- If you still see it, contact your system administrator
- The fix: book_cover field is now properly handled (set to NULL during import)

---

## Permissions

### Who Can Import?
- **Administrators**: Can import both books and students
- **Librarians**: Can import both books and students
- **POS Operators**: Cannot import
- **Students**: Cannot import

### Audit Trail
- All librarian imports are logged in the Admin Log
- Admins can view who imported what and when
- This provides accountability and tracking

---

*CSV Import Guide - Library Management System*
*Last Updated: October 22, 2025*
