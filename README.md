# 📊 Youtube Data Analizi və Vizuallaşdırılması

Bu hesabatda platformadakı fərqli məzmun kateqoriyalarının performansı və istifadəçi reaksiyaları qısa analitik bəndlərlə şərh olunmuşdur.

---

### 1. 📂 Kateqoriyalara Görə Paylaşma Sayı (Bar Chart)
* Platformada ən çox istehsal olunan və paylaşılan kontentlər **Food (Qida)** və **Tech (Texnologiya)** kateqoriyalarına aiddir. Ən az paylaşım sayı isə **Travel (Səyahət)** və **Lifestyle (Həyat tərzi)** qruplarında qeydə alınmışdır. Bu, platformadakı kontent təklifinin strukturunu göstərir.
  
 <img width="713" height="330" alt="y1" src="https://github.com/user-attachments/assets/214d101b-9c28-4f82-abb1-3e06fca46f10" />


```python
plt.figure(figsize=(8,3))
sns.countplot(x="category", data=df, color='yellow')
plt.title("Kateqoriyalara görə Paylaşma sayı")
plt.xlabel("Kateqoriya")
plt.ylabel("Paylaşma sayı")
plt.show()

```

---

### 2. 📈 İzlənmə Sayı Histogramı (Stacked Histogram)

* Videoların ümumi baxış paylanması göstərir ki, kütləvi sıxlıq **0 - 500,000** baxış aralığındadır və əsas pik nöqtəsi **300,000** ətrafındadır. **Tech** və **Comedy** demək olar ki, bütün baxış diapazonlarında stabil paylandığı halda, **Education** videoları daha çox aşağı baxışlı seqmentdə cəmləşmişdir.
  
<img width="885" height="542" alt="y2" src="https://github.com/user-attachments/assets/dae31381-b7d1-4f70-a70e-2321231746e2" />


```python
plt.figure(figsize=(10,6))
sns.histplot(
    data=df, 
    x='views', 
    hue='category', 
    multiple='stack',
    bins=20)
plt.title("İzlənmə Sayı Histogramı (Kateqoriya üzrə)")
plt.xlabel("İzlənmə Sayı")
plt.ylabel("Videoların sayı")
plt.show()

```

---

### 3. 🎯 Videolar Üzrə Qarşılıqlı Əlaqə Göstəricisi

* **Lifestyle (Həyat tərzi)** kateqoriyası **0.8-dən yüksək** nisbətlə istifadəçiləri ən çox cəlb edən və onlardan reaksiya alan məzmun növüdür. Əksinə, **Comedy** kateqoriyası çox izlənməsinə baxmayaraq, istifadəçilərlə dərin qarşılıqlı əlaqə (nisbət) qurmaqda ən aşağı performansı (~0.15) göstərir.
  
<img width="600" height="483" alt="y3" src="https://github.com/user-attachments/assets/3c20abc9-f5ce-46d5-8c42-523a584e900e" />

```python
df["rate"] = (df["likes"] + df["comments"] + df["shares"]) / df["views"]
df.groupby("category")["rate"].mean().sort_values(ascending=False)
df.groupby("category")["rate"].mean().plot(kind="bar")
plt.ylabel("Rate")
plt.show()

```

---

### 4. 📊 Kateqoriyalara Görə Bəyənmələr və Şərhlər (Grouped Bar Chart)

*  Bütün kateqoriyalarda bəyənmə (Likes) həcmi şərh (Comments) sayını kəskin şəkildə üstələyir. **Comedy** kateqoriyası 1.35 milyondan çox bəyənmə ilə platformanın vizual olaraq ən çox bəyənilən sahəsidir. **Travel** kateqoriyası isə həm bəyənmə, həm də şərh həcmində ən geridə qalan istiqamətdir.
  
<img width="872" height="491" alt="y4" src="https://github.com/user-attachments/assets/e91de686-36b3-40ae-be9e-c13bdcc9acd1" />

```python
category_stats = df.groupby("category")[["likes", "comments"]].sum()
category_stats.plot(kind="bar", figsize=(8,5))
plt.title("Kateqoriyalara görə Bəyənmələr və Şərhlər")
plt.xlabel("Kateqoriya")
plt.ylabel("Say")
plt.xticks(rotation=0)
plt.legend(["Likes", "Comments"])
plt.tight_layout()
plt.show()

```

---

### 5. 🌡️ Ədədi Dəyişənlər Arasındakı Korelasiya (Heatmap)

*  Video müddəti, baxış, bəyənmə, şərh və hashtag sayları arasındakı korelasiya əmsalları sıfıra çox yaxındır (**-0.05 ilə 0.08**). Bu, dəyişənlər arasında **xətti əlaqənin olmadığını** sübut edir. Yəni videonun uzun olması və ya çoxlu hashtag daşınması onun daha çox baxış və ya bəyənmə alacağına zəmanət vermir; performans fərqli amillərdən (məsələn, keyfiyyətdən) asılıdır.

<img width="822" height="528" alt="y5" src="https://github.com/user-attachments/assets/7db2a3e8-84b4-43b0-8f0c-6eed9c7a4e7b" />

```python
cols = ['duration_sec', 'views', 'likes', 'comments', 'shares', 'hashtags_count']

matrix = df[cols].corr()

plt.figure(figsize=(8,6))
sns.heatmap(matrix, annot=True, fmt=".2f",  cmap="coolwarm", cbar=True)
plt.title("Ədədi Dəyişənlər Arasındakı Korelasiya")
plt.show()

```
