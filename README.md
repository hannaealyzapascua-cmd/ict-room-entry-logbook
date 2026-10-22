
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, initial-scale=1.0">

  <title>ICT Room E-Logbook</title>

  <!-- CSS: Controls the design -->
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f4f8;
      margin: 0;
      padding: 20px;
    }

    .container {
      max-width: 700px;
      margin: auto;
      background: white;
      padding: 24px;
      border-radius: 12px;
    }

    h1 {
      color: #174a7e;
      text-align: center;
    }

    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
    }

    input, button {
      width: 100%;
      box-sizing: border-box;
      padding: 12px;
      margin-top: 8px;
      font-size: 16px;
    }

    button {
      background: #174a7e;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
    }

    th, td {
      border: 1px solid #ddd;
      padding: 8px;
      text-align: left;
    }

    th {
      background: #174a7e;
      color: white;
    }
  </style>
</head>

<body>
  <div class="container">
    <h1>ICT ROOM E-Logbook</h1>
    <h2>Entry E-Logbook</h2>

    <!-- HTML: The form -->
    <form id="entryForm">
      <label for="personName">Name</label>
      <input id="personName"
             type="text"
             placeholder="Enter full name"
             required>

      <label for="position">Grade Level / Position</label>
      <input id="position"
             type="text"
             placeholder="e.g. Grade 10 / Teacher"
             required>

      <button type="submit">Record Entry</button>
    </form>

    <h2>Entry Records</h2>

    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Grade / Position</th>
          <th>Entry Time</th>
        </tr>
      </thead>
      <tbody id="recordList"></tbody>
    </table>
  </div>

  <!-- JavaScript: Makes the form work -->
  <script>
    const form = document.getElementById("entryForm");
    const recordList =
      document.getElementById("recordList");

    // Load saved records from this browser
    let records = JSON.parse(
      localStorage.getItem("ictEntries") || "[]"
    );

    function displayRecords() {
      recordList.replaceChildren();

      records.forEach(function(record) {
        const row = document.createElement("tr");

        [record.name, record.position, record.time]
          .forEach(function(value) {
            const cell = document.createElement("td");
            cell.textContent = value;
            row.appendChild(cell);
          });

        recordList.appendChild(row);
      });
    }

    form.addEventListener("submit", function(event) {
      event.preventDefault();

      const name =
        document.getElementById("personName").value.trim();

      const position =
        document.getElementById("position").value.trim();

      if (!name || !position) return;

      const record = {
        name: name,
        position: position,
        time: new Date().toLocaleString()
      };

      records.push(record);

      localStorage.setItem(
        "ictEntries",
        JSON.stringify(records)
      );

      displayRecords();
      form.reset();

      alert("Entry recorded successfully!");
    });

    displayRecords();
  </script>
</body>
</html>
