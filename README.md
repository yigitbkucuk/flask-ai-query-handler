# flask-ai-query-handler
In this repository, you can find the code for an interface program I created. The code is written exclusively in Turkish and English. You can disable the language you don't want to use by commenting it out and run the program in your preferred language. The code is designed to allow easy switching between Turkish and English.

Bu depoda, kendi hazırlamış olduğum bir arayüz programına ait kodu bulabilirsiniz. Kod yalnızca Türkçe ve İngilizce olarak yazılmıştır. Kullanmak istemediğiniz dili yorum satırlarıyla devre dışı bırakabilir ve tercih ettiğiniz dilde çalıştırabilirsiniz. Kod, Türkçe ve İngilizce dilleri arasında kolayca geçiş yapabilmeniz için tasarlanmıştır.


# English 

from flask import Flask, request, render_template_string
import time
import string
import urllib.parse

app = Flask(__name__)

# CSS styled HTML template
html_template = """
<!DOCTYPE html>
<html>
<head>
    <title>YBK AI</title>
    <style>
        body {
            font-family: Times New Roman, sans-serif;
            background-image: url('/static/staticimagesyour-image-name (2).png');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            color: #333;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            padding: 40px;
            border-radius: 8px;
            background-color: rgba(255, 255, 255, 0.8);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            max-width: 500px;
            width: 100%;
            text-align: center;
        }
        h1 {
            color: #0073e6;
            font-size: 36px;
            margin-bottom: 20px;
        }
        p {
            font-size: 18px;
        }
        form {
            margin-top: 15px;
        }
        label {
            font-weight: bold;
            font-size: 18px;
            margin-bottom: 5px;
            display: block;
        }
        input[type="text"] {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #0073e6;
            color: white;
            font-size: 16px;
            border: none;
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #005bb5;
        }
        .hidden {
            visibility: hidden;
        }
    </style>
    <script>
        function showResponse(response) {
            const resultElement = document.getElementById('result');
            resultElement.textContent = '';
            let index = 0;
            const words = response.split(' ');
            function typeNextWord() {
                if (index < words.length) {
                    resultElement.textContent += words[index] + ' ';
                    index++;
                    setTimeout(typeNextWord, 250); // Typing speed for words
                }
            }
            typeNextWord();
        }

        function showLoading() {
            const resultElement = document.getElementById('result');
            resultElement.textContent = 'Waiting for response...';
        }
    </script>
</head>
<body>
    <div class="container">
        <h1>{{ heading }}</h1>
        <p>{{ intro_message }}</p>
        <form method="post" onsubmit="showLoading()">
            <label for="query"> </label>
            <input type="text" id="query" name="query" required>
            <button type="submit">Submit</button>
        </form>
        {% if response %}
        <h2>Response:</h2>
        <p id="result" class="hidden">{{ response }}</p>
        <script>
            setTimeout(function() {
                document.getElementById('result').classList.remove('hidden');
                showResponse("{{ response|escape }}");
            }, 3000); // 3-second delay
        </script>
        {% endif %}
        {% if redirect_link %}
        <h2>For more information:</h2>
        <p><a href="{{ redirect_link }}" target="_blank">Search here</a></p>
        {% endif %}
    </div>
</body>
</html>
"""

answers = {
    "what is artificial intelligence": "Artificial Intelligence (AI) is a multidisciplinary field that involves creating systems or machines that simulate human intelligence. It combines computer science, mathematics, statistics, neuroscience, and even philosophy. For more information, click the link below.",
    "what are the types of artificial intelligence": "Artificial intelligence is classified into various types based on its characteristics and applications. 1. Weak AI: Designed for specific tasks (e.g., Siri). 2. Unsupervised Learning: Finds patterns in unlabeled data (e.g., customer segmentation). 3. Reinforcement Learning: Learns through rewards and penalties (e.g., AlphaGo). For more details, check the link below.",
    "what is llm": "LLMs are language models trained on large datasets. These models learn semantic relationships and contextual usage. For more information, click the link below.",
    "hello": "Hello! How can I assist you?",
    "goodbye": "Goodbye! Have a great day!",
    "how are you": "I’m an AI assistant, so I don’t have feelings, but I’m here to help you!",
    "how's the weather": "You can get weather information based on your location.",
    "what time is it": "I don’t have access to real-time clock data, but you can use a clock application.",
    "thank you": "You’re welcome! Happy to assist.",
}

redirects = {
    "what is artificial intelligence": "https://en.wikipedia.org/wiki/Artificial_intelligence",
    "what are the types of artificial intelligence": "https://en.wikipedia.org/wiki/Types_of_artificial_intelligence",
    "what is llm": "https://en.wikipedia.org/wiki/Language_model"
}

@app.route('/', methods=['GET', 'POST'])
def index():
    heading = "Welcome"
    intro_message = "Please enter your query and see the result."
    response = None
    redirect_link = None

    if request.method == 'POST':
        query = request.form.get('query', '').lower().strip()
        query = query.translate(str.maketrans('', '', string.punctuation))
        response = answers.get(query, None)
        time.sleep(3)  # Delay for response simulation
        if query in redirects:
            redirect_link = redirects[query]
        if response is None:
            google_query = urllib.parse.quote_plus(query)
            redirect_link = f"https://www.google.com/search?q={google_query}"
            response = "I can redirect you to the link below for this topic."
    return render_template_string(html_template, heading=heading, intro_message=intro_message, response=response, redirect_link=redirect_link)

if __name__ == '__main__':
    app.run(debug=True)



# Turkish

from flask import Flask, request, render_template_string
import time
import string
import urllib.parse

app = Flask(__name__)

# CSS eklenmiş HTML şablonu
html_template = """
<!DOCTYPE html>
<html>
<head>
    <title>YBK AI</title>
    <style>
        body {
            font-family: Times New Roman, sans-serif;
            background-image: url('/static/staticimagesyour-image-name (2).png');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            color: #333;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            padding: 40px;
            border-radius: 8px;
            background-color: rgba(255, 255, 255, 0.8);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            max-width: 500px;
            width: 100%;
            text-align: center;
        }
        h1 {
            color: #0073e6;
            font-size: 36px;
            margin-bottom: 20px;
        }
        p {
            font-size: 18px;
        }
        form {
            margin-top: 15px;
        }
        label {
            font-weight: bold;
            font-size: 18px;
            margin-bottom: 5px;
            display: block;
        }
        input[type="text"] {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #0073e6;
            color: white;
            font-size: 16px;
            border: none;
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #005bb5;
        }
        .hidden {
            visibility: hidden;
        }
    </style>
    <script>
        function showResponse(response) {
            const resultElement = document.getElementById('result');
            resultElement.textContent = '';
            let index = 0;
            const words = response.split(' ');
            function typeNextWord() {
                if (index < words.length) {
                    resultElement.textContent += words[index] + ' ';
                    index++;
                    setTimeout(typeNextWord, 250); // Harflerin açılma hızı
                }
            }
            typeNextWord();
        }

        function showLoading() {
            const resultElement = document.getElementById('result');
            resultElement.textContent = 'Yanıt Bekleniyor...';
        }
    </script>
</head>
<body>
    <div class="container">
        <h1>{{ heading }}</h1>
        <p>{{ intro_message }}</p>
        <form method="post" onsubmit="showLoading()">
            <label for="query"> </label>
            <input type="text" id="query" name="query" required>
            <button type="submit">Gönder</button>
        </form>
        {% if response %}
        <h2>Sonuç:</h2>
        <p id="result" class="hidden">{{ response }}</p>
        <script>
            setTimeout(function() {
                document.getElementById('result').classList.remove('hidden');
                showResponse("{{ response|escape }}");
            }, 3000); // 3 saniye gecikme
        </script>
        {% endif %}
        {% if redirect_link %}
        <h2>Daha fazla bilgi için:</h2>
        <p><a href="{{ redirect_link }}" target="_blank">Burada ara</a></p>
        {% endif %}
    </div>
</body>
</html>
"""

answers = {
    "yapay zeka nedir": "Yapay Zeka (AI), insan zekasını simüle eden sistemlerin veya makinelerin oluşturulmasını içeren disiplinler arası bir bilim alanıdır. Bilgisayar bilimleri, matematik, istatistik, sinirbilim ve hatta felsefenin bir kombinasyonu olarak gelişmiştir. Daha fazla bilgi almak isterseniz aşağıdaki bağlantıya tıklayın.",
    "yapay zeka türleri nelerdir": "Yapay zeka farklı özelliklere ve kullanım alanlarına göre çeşitli türlere ayrılır. 1. Zayıf Yapay Zeka (Weak AI): Belirli bir görev için tasarlanmıştır. Örnek: Siri. 2. Denetimsiz Öğrenme: Sistem etiketsiz verilerle desenler tanır. Örnek: Müşteri segmentasyonu. 3. Pekiştirmeli Öğrenme: Sistem ödül ve cezalarla öğrenir. Örnek: AlphaGo. Daha fazla bilgi almak için aşağıdaki bağlantıyı inceleyebilirsiniz.",
    "llm nedir": "LLM, büyük veri kümesi üzerinde eğitilmiş dil modelleridir. Bu modeller anlam ilişkilerini ve bağlamsal kullanımları öğrenir. Daha fazla bilgi için aşağıdaki bağlantıya tıklayın.",
    "merhaba": "Merhaba! Size nasıl yardımcı olabilirim?",
    "görüşürüz": "Görüşürüz! İyi günler dilerim.",
    "nasılsın": "Ben bir yapay zeka asistanıyım, hislerim yok ama size yardımcı olmaktan mutluyum!",
    "hava nasıl": "Hava durumu için bulunduğunuz konuma göre bilgi alabilirsiniz.",
    "saat kaç": "Şu an saat bilgisine erişimim yok ama bir saat uygulamasını kullanabilirsiniz.",
    "teşekkür ederim": "Rica ederim! Size yardımcı olmaktan mutluluk duydum.",
}

redirects = {
    "yapay zeka nedir": "https://tr.wikipedia.org/wiki/Yapay_zeka",
    "yapay zeka türleri nelerdir": "https://tr.wikipedia.org/wiki/Yapay_zekanın_türleri",
    "llm nedir": "https://tr.wikipedia.org/wiki/Dil_modeli"
}

@app.route('/', methods=['GET', 'POST'])
def index():
    heading = "Hoşgeldiniz"
    intro_message = "Lütfen öğrenmek istediğiniz şeyi girin ve sonucu görün."
    response = None
    redirect_link = None

    if request.method == 'POST':
        query = request.form.get('query', '').lower().strip()
        query = query.translate(str.maketrans('', '', string.punctuation))
        response = answers.get(query, None)
        time.sleep(3)  # Yanıt gecikmesi için bekleme
        if query in redirects:
            redirect_link = redirects[query]
        if response is None:
            google_query = urllib.parse.quote_plus(query)
            redirect_link = f"https://www.google.com/search?q={google_query}"
            response = "Bu konu için sizi aşağıdaki linke yönlendirebilirim."
    return render_template_string(html_template, heading=heading, intro_message=intro_message, response=response, redirect_link=redirect_link)

if __name__ == '__main__':
    app.run(debug=True)


