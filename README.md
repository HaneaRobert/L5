<!DOCTYPE html>
<html lang="ro">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Căutare Țări</title>
    <style>
        body {
            font-family: 'Verdana', sans-serif;
            background-color: #eef2f3;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            max-width: 500px;
            background: #fff;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 2px 5px 10px rgba(0, 0, 0, 0.15);
        }
        .search-input {
            width: 100%;
            padding: 12px;
            font-size: 14px;
            border: 2px solid #bbb;
            border-radius: 8px;
            outline: none;
        }
        .country-item {
            display: flex;
            align-items: center;
            padding: 8px;
            border-bottom: 1px solid #ccc;
        }
        .country-item img {
            width: 35px;
            height: 22px;
            margin-right: 10px;
            border: 1px solid #999;
        }
        .loading {
            font-style: italic;
            color: gray;
        }
    </style>
    <script>
        let debounceTimer;

        const fetchCountries = async () => {
            clearTimeout(debounceTimer);
            debounceTimer = setTimeout(async () => {
                let input = document.getElementById("search-input").value.trim();
                let resultsDiv = document.getElementById("results");
                resultsDiv.innerHTML = "<p class='loading'>Căutare...</p>";

                if (input === "") {
                    resultsDiv.innerHTML = "<p>Introduceți un nume de țară!</p>";
                    return;
                }

                try {
                    const res = await fetch(`https://restcountries.com/v3.1/name/${input}`);
                    if (!res.ok) throw new Error("Nu am găsit această țară.");
                    const data = await res.json();
                    
                    resultsDiv.innerHTML = `<p>${data.length} rezultate găsite:</p>`;
                    data.forEach(c => {
                        resultsDiv.innerHTML += `
                            <div class="country-item">
                                <img src="${c.flags.svg}" alt="Steagul ${c.name.common}">
                                <p><strong>${c.name.common}</strong> - Populație: ${c.population.toLocaleString()}</p>
                            </div>`;
                    });
                } catch (err) {
                    resultsDiv.innerHTML = `<p style="color:red;">Eroare: ${err.message}</p>`;
                }
            }, 300);
        };
    </script>
</head>
<body>
    <div class="container">
        <h2>Căutare țări în timp real</h2>
        <input type="text" id="search-input" class="search-input" placeholder="Introduceți numele unei țări" oninput="fetchCountries()">
        <div id="results"></div>
    </div>
</body>
</html>
