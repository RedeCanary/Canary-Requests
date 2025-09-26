# Rede Canary - Requests
> Não recomendada para uso oficial no momento!

🧩 Este repositório serve para desenvolvedores ou curiosos requisitarem informações constantes do SKY BLOCK de maneira rápida,
os recursos das requisições são atualizados a cada modificação feita por nós.

🧶 **Destrinchando as requisições**
As requisições podem ser usadas para quaisquers finalidades:
  - Clientes de Minecraft
  - Criação de (plugins/mods/sites)
  - Criação de bibliotecas
  - E muito mais!

🪡 **Com a agulha na mão**
Abaixo há exemplos de como fazer usos de requisições em algumas
linguagens de programação comumente usadas:

🐍 **Python**
  Requisitos:
    - Biblioteca requests
    - Python 3.0 ou superior
```python
import requests

def main():
    try:
        response = requests.get("https://raw.githubusercontent.com/RedeCanary/Canary-Requests/refs/heads/main/skyblock/enchants.json")
        response.raise_for_status()
        data = response.json()
        
        for enchant in data:
            print("ID: {}\nNome: {}\nConflitos: {}".format(
                data[enchant]["id"], 
                data[enchant]["name"], 
                data[enchant]["extra"]["conflicts"]
            ))

    except Exception as e:
        print(f"Ocorreu um erro: {e}")

if __name__ == "__main__":
    main()
```

☕ **Java**
  Requisitos:
    - Biblioteca GSON (Google)
    - Framework Lombok
    - Java 8 ou superior
```java
public class Main {

    public static void main(String[] args) {
        try {
            final String URL_LINK = "https://raw.githubusercontent.com/RedeCanary/Canary-Requests/refs/heads/main/skyblock/enchants.json";

            String content = getContent(URL_LINK);

            Gson gson = new Gson();
            Map<String, Object> rawData = gson.fromJson(content, Map.class);

            for (String enchantKey : rawData.keySet()) {
                Map<String, Object> enchantData = (Map<String, Object>) rawData.get(enchantKey);

                Double id = (Double) enchantData.get("id");
                String name = (String) enchantData.get("name");

                Map<String, Object> extra = (Map<String, Object>) enchantData.get("extra");
                List<String> conflicts = (List<String>) extra.get("conflicts");

                System.out.println("ID: " + id.intValue());
                System.out.println("Nome: " + name);
                System.out.println("Conflitos: " + conflicts);
                System.out.println("-----------------------------------");
            }

        } catch (Exception e) {
            System.out.println("Ocorreu um erro: " + e.getMessage());
            e.printStackTrace();
        }
    }

    private static String getContent(String urlLink) throws IOException {
        URL url = new URL(urlLink);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestMethod("GET");

        BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
        String inputLine;
        StringBuilder content = new StringBuilder();

        while ((inputLine = in.readLine()) != null) {
            content.append(inputLine);
        }

        in.close();
        connection.disconnect();

        return content.toString();
    }
}

```

⚙️ **Rust**
  Requisitos:
    - Biblioteca serde/serde_Json
    - Biblioteca reqwest/tokio
    - Rust nightly ou outro
```rust
use std::collections::HashMap;
use reqwest::Error;
use serde::Deserialize;

#[derive(Debug, Deserialize)]
struct EnchantExtra {
    conflicts: Vec<String>,
}

#[derive(Debug, Deserialize)]
struct Enchant {
    id: u32,
    name: String,
    extra: EnchantExtra,
}

#[tokio::main]
async fn main() -> Result<(), Error> {
    let url = "https://raw.githubusercontent.com/RedeCanary/Canary-Requests/refs/heads/main/skyblock/enchants.json";

    let response = reqwest::get(url).await?;
    let data: HashMap<String, Enchant> = response.json().await?;

    for (enchant_name, enchant) in data {
        println!(
            "ID: {}\nNome: {}\nConflitos: {:?}\n",
            enchant.id, enchant.name, enchant.extra.conflicts
        );
    }

    Ok(())
}
```
