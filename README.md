# Creative-Tech-Project3_Pokemon
<!DOCTYPE html>
<html>
  <head>
    <title>My Pokemon Collection</title>
    <style>
      body {
        font-family: "Arial", sans-serif;
        background-color: #f4f4f4;
        margin: 0;
        padding: 20px;
        text-align: center;
      }

      label {
        font-size: 18px;
        margin-right: 10px;
      }

      input {
        padding: 8px;
        font-size: 16px;
      }

      button {
        padding: 10px;
        font-size: 16px;
        background-color: #4caf50;
        color: white;
        border: none;
        cursor: pointer;
      }

      button:hover {
        background-color: #45a049;
      }

      #pokemonList {
        margin-top: 20px;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
      }

      #pokemonList a {
        margin: 10px;
        padding: 10px;
        background-color: #3498db;
        color: white;
        text-decoration: none;
        border-radius: 5px;
        transition: background-color 0.3s ease;
        line-height: 4;
      }

      #pokemonList a:hover {
        background-color: #2980b9;
      }

      #cryptodata {
        margin-top: 20px;
        padding: 20px;
        background-color: #fff;
        border-radius: 5px;
      }

      #cryptochart {
        margin-top: 20px;
        padding: 20px;
        background-color: #fff;
        border-radius: 5px;
      }
    </style>
  </head>

  <body>
    <img src="Project1 Week3.png" alt="Documentation" style="width:100%;max-width:300px">
    <label for="numberOfPokemons">Number of Pokemons:</label>
    <input
      type="number"
      id="numberOfPokemons"
      placeholder="Enter the number of pokemons"
    />
    <button onclick="getPokemenData()">Get Pokemon Data</button>

    <div id="pokemonList">
      <!-- Pokemon names will be placed here -->
    </div>

    <div id="cryptodata">
      <!-- Pokemon details will be placed here -->
    </div>

    <div id="cryptochart">
      <!-- Ability details will be placed here -->
    </div>

    <script>
      function getPokemenData() {
        var numberOfPokemons =
          document.getElementById("numberOfPokemons").value;

        if (!numberOfPokemons || numberOfPokemons <= 0) {
          alert("Please enter a valid number of pokemons.");
          return;
        }

        var pokemonList = document.getElementById("pokemonList");
        pokemonList.innerHTML = ""; // Clear previous data

        fetch(
          `https://pokeapi.co/api/v2/pokemon?limit=${numberOfPokemons}&offset=0`
        )
          .then((response) => {
            if (response.ok) {
              return response.json();
            } else {
              throw new Error("Network response was not ok.");
            }
          })
          .then((data) => {
            data.results.forEach((pokemon, index) => {
              pokemonList.innerHTML += `<div><a href="#" onclick="getPokemonDetails(${
                index + 1
              })">${pokemon.name}</a></div>`;
            });
          })
          .catch((error) => {
            document.getElementById("pokemonList").innerText =
              "Error fetching Pokemon list: " + error.message;
          });
      }

      function getPokemonDetails(pokemonId) {
        var pokemonUrl = `https://pokeapi.co/api/v2/pokemon/${pokemonId}/`;

        fetch(pokemonUrl)
          .then((response) => {
            if (response.ok) {
              return response.json();
            } else {
              throw new Error("Network response was not ok.");
            }
          })
          .then((data) => {
            const abilities = data.abilities.map((ability) => {
              return `<div><a href="#" onclick="getAbilityDetails('${ability.ability.url}')">${ability.ability.name}</a></div>`;
            });

            document.getElementById(
              "cryptodata"
            ).innerHTML = `<div><b>Pokemon Name:</b> ${data.name}</div>`;
            document.getElementById(
              "cryptodata"
            ).innerHTML += `<div><b>Abilities:</b> ${abilities.join("")}</div>`;
          })
          .catch((error) => {
            document.getElementById("cryptodata").innerText =
              "Error fetching Pokemon data: " + error.message;
          });
      }

      function getAbilityDetails(abilityUrl) {
        fetch(abilityUrl)
          .then((response) => {
            if (response.ok) {
              return response.json();
            } else {
              throw new Error("Network response was not ok.");
            }
          })
          .then((abilityData) => {
            document.getElementById(
              "cryptochart"
            ).innerHTML = `<div><b>Ability Name:</b> ${abilityData.name}</div>`;
            document.getElementById(
              "cryptochart"
            ).innerHTML += `<div><b>Effect:</b> ${abilityData.effect_entries[0].effect}</div>`;
          })
          .catch((error) => {
            document.getElementById("cryptochart").innerText =
              "Error fetching Ability data: " + error.message;
          });
      }
    </script>
  </body>
</html>
