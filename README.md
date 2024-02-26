# PDF-Data-Extraction-and-Rapid-Prototyping
#Given an invoice PDF, your objective is to write a Python script that extracts key-value pairs from the document and saves the results to a CSV file. The PDF will include both header data and table data. Your script should be able to handle both types of data.
mport PyPDF2
import csv

def extract_key_value_pairs(pdf_path):
    with open(pdf_path, 'rb') as file:
        reader = PyPDF2.PdfFileReader(file)
        header_data = {}
        tabular_data = []

        for page_num in range(reader.numPages):
            page = reader.getPage(page_num)
            text = page.extractText()

            # Assuming headers are in the format "Key: Value"
            header_lines = [line.strip() for line in text.split('\n') if ':' in line]
            header_data.update(dict(line.split(':') for line in header_lines))

            # Assuming tabular data is present in a specific format
            # Modify this section based on your actual tabular data structure
            # Here, it's assumed to be in a CSV-like format
            rows = [line.strip().split(',') for line in text.split('\n') if ',' in line]
            tabular_data.extend(rows)

    return header_data, tabular_data

def save_to_csv(header_data, tabular_data, csv_path):
    with open(csv_path, 'w', newline='') as csv_file:
        writer = csv.writer(csv_file)
        
        # Write headers
        writer.writerow(header_data.keys())
        writer.writerow(header_data.values())

        # Write tabular data
        writer.writerows(tabular_data)

if __name__ == "__main__":
    pdf_path = 'path/to/your/invoice.pdf'
    csv_path = 'output.csv'

    header_data, tabular_data = extract_key_value_pairs(pdf_path)
    save_to_csv(header_data, tabular_data, csv_path)
