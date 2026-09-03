<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Gerador de Senhas</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="card">
      <h2>Gerador de Senhas</h2>

      <div class="display-group">
        <input
          type="text"
          id="password"
          class="password-input"
          readonly
          placeholder="Sua senha"
        />
        <button id="copy-btn" class="btn-copy" title="Copiar">
          <svg
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
          >
            <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
            <path
              d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"
            ></path>
          </svg>
        </button>
      </div>

      <div class="strength-meter">
        <div id="strength-bar" class="strength-bar"></div>
      </div>

      <div class="options-container">
        <div class="range-box">
          <div class="range-info">
            <span>Tamanho</span>
            <span id="length-val">12</span>
          </div>
          <input type="range" id="length" min="6" max="32" value="12" />
        </div>

        <label class="checkbox-box">
          <span>Incluir Maiúsculas</span>
          <input type="checkbox" id="uppercase" checked />
        </label>

        <label class="checkbox-box">
          <span>Incluir Minúsculas</span>
          <input type="checkbox" id="lowercase" checked />
        </label>

        <label class="checkbox-box">
          <span>Incluir Números</span>
          <input type="checkbox" id="numbers" checked />
        </label>

        <label class="checkbox-box">
          <span>Incluir Símbolos</span>
          <input type="checkbox" id="symbols" checked />
        </label>
      </div>

      <button id="generate-btn" class="btn-generate">Gerar Senha</button>
    </div>
    <script>
      const passwordInput = document.getElementById("password");
      const copyBtn = document.getElementById("copy-btn");
      const lengthInput = document.getElementById("length");
      const lengthVal = document.getElementById("length-val");
      const uppercaseCb = document.getElementById("uppercase");
      const lowercaseCb = document.getElementById("lowercase");
      const numbersCb = document.getElementById("numbers");
      const symbolsCb = document.getElementById("symbols");
      const generateBtn = document.getElementById("generate-btn");
      const strengthBar = document.getElementById("strength-bar");

      const chars = {
        uppercase: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
        lowercase: "abcdefghijklmnopqrstuvwxyz",
        numbers: "0123456789",
        symbols: "!@#$%^&*()_+-=[]{}|;:,.<>?",
      };

      lengthInput.addEventListener("input", () => {
        lengthVal.textContent = lengthInput.value;
      });

      function updateStrength(length, typeCount) {
        let score = length * typeCount;
        if (score < 20) {
          strengthBar.style.width = "25%";
          strengthBar.style.background = "#ef4444"; // Vermelho (Fraca)
        } else if (score < 40) {
          strengthBar.style.width = "60%";
          strengthBar.style.background = "#f59e0b"; // Amarelo (Média)
        } else {
          strengthBar.style.width = "100%";
          strengthBar.style.background = "#10b981"; // Verde (Forte)
        }
      }

      function generatePassword() {
        let availableChars = "";
        let typeCount = 0;

        if (uppercaseCb.checked) {
          availableChars += chars.uppercase;
          typeCount++;
        }
        if (lowercaseCb.checked) {
          availableChars += chars.lowercase;
          typeCount++;
        }
        if (numbersCb.checked) {
          availableChars += chars.numbers;
          typeCount++;
        }
        if (symbolsCb.checked) {
          availableChars += chars.symbols;
          typeCount++;
        }

        if (availableChars === "") {
          alert("Selecione pelo menos uma opção!");
          return;
        }

        let password = "";
        const length = parseInt(lengthInput.value);

        for (let i = 0; i < length; i++) {
          const randomIndex = Math.floor(Math.random() * availableChars.length);
          password += availableChars[randomIndex];
        }

        passwordInput.value = password;
        updateStrength(length, typeCount);
      }

      copyBtn.addEventListener("click", () => {
        if (!passwordInput.value) return;
        navigator.clipboard.writeText(passwordInput.value);
        alert("Senha copiada!");
      });

      generateBtn.addEventListener("click", generatePassword);

      // Gerar uma senha ao carregar a página
      generatePassword();
    </script>
  </body>
</html>
