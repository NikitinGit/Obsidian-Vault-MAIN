1. ssh agent - это прога по работе с ссш ключами. При подключение к серверу автоматически подставляет ключ  ^4d50b7
2. ### **Отличие публичного (открытого) ключа от приватного (закрытого) в SSH**

В SSH используется **асимметричная криптография**, где ключи работают в паре:

| Характеристика             | **Приватный ключ (private key)**                | **Публичный ключ (public key)**                       |
| -------------------------- | ----------------------------------------------- | ----------------------------------------------------- |
| **Где хранится?**          | Только у владельца (на локальном ПК, защищён).  | Можно свободно передавать (на сервер, в Git и т. д.). |
| **Доступ**                 | Должен быть строго защищён (например, паролем). | Открыто распространяется.                             |
| **Формат**                 | Обычно `id_rsa`, `id_ed25519` (без `.pub`).     | Имеет расширение `.pub` (например, `id_rsa.pub`).     |
| **Можно ли восстановить?** | Нет! Если потерян — доступ пропадает.           | Можно сгенерировать заново из приватного.             |
| **Функция**                | **Подпись и расшифровка** данных.               | **Шифрование и проверка подписи**.                    |

---

### **Пример работы SSH-ключей**

1. **Подключение к серверу:**
    
    - Сервер проверяет, что публичный ключ (`~/.ssh/authorized_keys`) соответствует приватному.
        
    - Если совпадение есть — доступ разрешён.
        
2. **Git (GitHub/GitLab):**
    
    - Вы загружаете `id_rsa.pub` в настройки аккаунта.
        
    - При `git push` система проверяет подпись вашего приватного ключа.
        

---

### **Как выглядит ключ?**

- **Приватный (`id_rsa`):**
    
    text
    
    Copy
    
    -----BEGIN OPENSSH PRIVATE KEY-----
    b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAA...
    
- **Публичный (`id_rsa.pub`):**
    
    text
    
    Copy
    
    ssh-rsa AAAAB3NzaC1yc2E... user@host
    

---

### **Главное правило безопасности**

🔒 **Приватный ключ — как пароль!** Никому не передавать, не коммитить в Git, хранить в `~/.ssh/` с правами `600`.

🔓 **Публичный ключ — можно раздавать** серверам, Git-хостам и коллегам (если нужен доступ).

---

### **Как проверить ключи?**

bash

Copy

# Показать fingerprint публичного ключа
ssh-keygen -lf ~/.ssh/id_rsa.pub

# Проверить, работает ли связка
ssh -T git@github.com  # Для GitHub

---

### **Вывод**

- **Приватный ключ** — ваш «секретный пароль», который **никогда не должен передаваться**.
    
- **Публичный ключ** — открытая часть, которая **доказывает**, что у вас есть приватный ключ.
    

Если нужно сгенерировать новую пару:

bash

Copy

ssh-keygen -t ed25519 -C "your_email@example.com"