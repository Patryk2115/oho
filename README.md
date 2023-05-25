import requests
from bs4 import BeautifulSoup


url = "https://fortnitetracker.com/" \



response = requests.get(url)


if response.status_code == 200:

    soup = BeautifulSoup(response.content, "html.parser")


    table = soup.find("table")

    if table:

        headers = [header.get_text(strip=True) for header in table.find_all("th")]


        rows = []
        for row in table.find_all("tr"):
            cells = row.find_all(["th", "td"])
            row_data = [cell.get_text(strip=True) for cell in cells]
            rows.append(row_data)

        
        max_lengths = [max(len(str(row[i])) for row in rows) for i in range(len(headers))]
        print(" | ".join(header.ljust(max_lengths[i]) for i, header in enumerate(headers)))
        print("-+-".join("-" * length for length in max_lengths))
        for row in rows:
            print(" | ".join(cell.ljust(max_lengths[i]) for i, cell in enumerate(row)))
    else:
        print("nie znaleziono .")
else:
    print("Błąd: nie masz dostępu.")
