# phoneomatic Android phonhoneeone
import requests
from bs4 import BeautifulSoup

print("🔍 स्किल डिमांड और रिव्यू ट्रैकर 🔍\n")

# जिस स्किल की आप डिमांड देखना चाहते हैं
skill = input("जिस स्किल के बारे में जानना चाहते हैं (जैसे python / dart): ").strip().lower()

# GitHub पर इस स्किल से जुड़े प्रोजेक्ट्स और ट्रेंड्स का लाइव पन्ना
url = f"https://github.com/topics/{skill}"

try:
    print(f"\n⏳ इंटरनेट से '{skill}' की लाइव डिमांड और रिव्यू मंगाए जा रहे हैं...")
    
    headers = {"User-Agent": "Mozilla/5.0"}
    response = requests.get(url, headers=headers)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # 1. स्किल का विवरण (Description)
        desc = soup.find('p', class_='color-fg-muted')
        if desc:
            print(f"\n📌 स्किल का विवरण:\n{desc.text.strip()}\n")
            
        # 2. इस स्किल के लोकप्रिय प्रोजेक्ट्स और उनके स्टार्स (रिव्यू / डिमांड का पैमाना)
        print("🔥 इस स्किल के टॉप प्रोजेक्ट्स और लोकप्रियता (Stars/Reviews):")
        print("-" * 50)
        
        articles = soup.find_all('article', class_='border', limit=5)
        
        for i, article in enumerate(articles, 1):
            # रिपॉजिटरी / प्रोजेक्ट का नाम
            repo_name = article.find('h3')
            repo_text = " ".join(repo_name.text.split()) if repo_name else "N/A"
            
            # स्टार्स (Ratings / Reviews)
            stars = article.find('span', id=lambda x: x and x.startswith('repo-stars-counter'))
            star_count = stars.text.strip() if stars else "N/A"
            
            print(f"{i}. प्रोजेक्ट: {repo_text}")
            print(f"   ⭐ स्टार्स / रिव्यू (Rating): {star_count}")
            print("-" * 50)
            
    else:
        print("❌ इस स्किल का डेटा नहीं मिल पाया। कृपया स्पेलिंग चेक करें।")

except Exception as e:
    print("❌ कनेक्शन में एरर आया:", e)
    honeneenee
one
