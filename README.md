# Sinf daftari

O‘qituvchi uchun davomat va baholarni yuritish, o‘quvchilar uchun esa o‘z natijalarini ko‘rish prototipi. Ilova bitta `index.html` faylida ishlaydi; server, paket o‘rnatish yoki internet aloqasi kerak emas.

## Kompyuterda ochish

1. `index.html` faylini Chrome, Edge yoki boshqa zamonaviy brauzerda oching.
2. O‘qituvchi sifatida kiring.
3. Birinchi kirishda parol almashtiriladi.
4. O‘quvchi qo‘shing yoki **Ma’lumotlar → Demo ma’lumot yaratish** orqali sinov sinfini yarating.

Brauzerning mahalliy xotirasi fayl manzillari uchun cheklangan bo‘lsa, `index.html` faylini GitHub Pages orqali oching.

## GitHub Pages’ga joylash

1. GitHub’da yangi ommaviy yoki shaxsiy repository yarating.
2. `index.html`, `README.md`, `sample-import.csv` va `sample-curriculum.csv` fayllarini repository’ning asosiy (`main`) tarmog‘iga yuklang.
3. Repository sahifasida **Settings → Pages** bo‘limini oching.
4. **Build and deployment** ichida **Deploy from a branch** ni tanlang.
5. Branch sifatida `main`, papka sifatida `/(root)` ni belgilang va **Save** tugmasini bosing.
6. GitHub Pages bergan manzilni oching. Yangilanishlar repository’ga yuborilgach avtomatik joylanadi.

Har bir o‘quvchi va o‘qituvchi brauzeri o‘zining alohida mahalliy nusxasini saqlaydi. GitHub Pages bu prototipdagi foydalanuvchilar o‘rtasida ma’lumot almashmaydi.

## O‘qituvchi parolini almashtirish

O‘qituvchi sifatida kiring, **Sozlamalar** bo‘limini oching, joriy parol va yangi parolni ikki marta kiriting va **Parolni yangilash** tugmasini bosing. Yangi parol kamida 6 belgidan iborat bo‘lsin. Birinchi kirishda parol almashtiriladi.

Parol xeshi SHA-256 bilan hisoblanadi. `crypto.subtle` mavjud bo‘lmagan mahalliy brauzerlarda ilova ichidagi SHA-256 zaxira hisoblagichi ishlaydi.

## Reyting og‘irliklarini o‘zgartirish

**Sozlamalar** bo‘limida o‘rtacha baho ko‘paytuvchisi, sababsiz kelmagan kun jarimasi va darsga kirmagan dars jarimasini o‘zgartiring. Boshlang‘ich qiymatlar mos ravishda `20`, `3` va `2`. Formula:

```text
ball = (o‘rtacha baho yoki 0) × ko‘paytuvchi
       − sababsiz kelmagan kunlar × kun jarimasi
       − kirmagan darslar × dars jarimasi
```

Sababli kelmagan kun uchun jarima yo‘q. Hafta dushanbadan boshlanadi, baholar shu haftadagi oddiy arifmetik o‘rtacha bilan hisoblanadi.

## CSV baho importi

`sample-import.csv` faylida 10 ta namuna qator bor. Import qilinadigan CSV faylida quyidagi sarlavha bo‘lishi shart:

```csv
student_username,subject,grade,date
```

`student_username` tizimdagi o‘quvchi loginiga, `subject` esa ilovadagi fan nomlaridan biriga to‘liq mos bo‘lishi kerak. `grade` joriy sozlamalardagi baho oralig‘ida butun son, `date` esa `YYYY-MM-DD` formatida yoziladi. Ixtiyoriy `comment` ustunini ham qo‘shish mumkin. Noto‘g‘ri qatorlar raqami va sababi importdan keyin sahifada ko‘rsatiladi.

Baholarni va davomatni **Ma’lumotlar** bo‘limidan alohida CSV fayllariga chiqarish mumkin. JSON zaxira nusxasi barcha ma’lumotlarni, sozlamalarni va haftalik yopilgan natijalarni saqlaydi.

## JSON ma’lumot importi

Dasturchi oynasidagi **Ma’lumotlar → JSON import** bo‘limida faylni tanlang. Import `classes`, `students`, `subjects`, `curriculum`, `schedule`, `grades` va `attendance` bo‘limlarini qabul qiladi; eski zaxira faylidagi `timetable` kaliti ham `schedule` sifatida taniladi. Faylda bo‘lmagan bo‘limlar mavjud ma’lumotni o‘zgarishsiz qoldiradi.

Saqlashdan oldin **Qo‘shish** (mavjud yozuvlarni yangilab, yangilarini qo‘shish) yoki **Almashtirish** (faylda bor bo‘limlarni almashtirish) usulini tanlang. Almashtirish mavjud yozuvlarga ta’sir qilsa, tasdiq so‘raladi. Hisobot qo‘shilgan, yangilangan va rad etilgan yozuvlar sonini hamda har bir rad sababini ko‘rsatadi. Import sinf darajalarini 1–11 oralig‘ida, shu darajadagi sinf nomlarini va o‘quvchi loginlarini takrorlanmasligini hamda o‘quv reja, jadval va baholardagi fan identifikatorlarini tekshiradi.

O‘quvchi yozuvida `password` maydoni bo‘lsa, import uni SHA-256 bilan xeshlab, faqat `passwordHash` qiymatini saqlaydi. Faylda `passwordHash` allaqachon bo‘lsa, u o‘zgarishsiz olinadi. JSON import faqat fayl tanlash tugmasi orqali boshlanadi.

## Reytingni sinash

Brauzer konsolida `window.runRatingTests()` buyrug‘ini bajaring. U bahosiz hafta, teng ballar, 83 ballik ishlangan misol va faqat sababli kelmagan kunlar uchun to‘rtta kichik tekshiruvni chiqaradi.

O‘quv reja tekshiruvlari uchun `window.runCurriculumTests()` buyrug‘ini bajaring.

## Sinflar va bosqichlar

Yangi o‘rnatishda 1–11-darajalarning har biri uchun `A`, `B`, `D`, `E` guruhlari yaratiladi (jami 44 ta, masalan `5-A` va `5-B`). Eski ma’lumotlar yangilanganda bor sinflar saqlanadi va har bir darajaga yetishmayotgan `B`, `D`, `E` qo‘shiladi. Mavjud `5-A` nomi saqlanadi, darajasi 5 deb belgilanadi. Migratsiya qayta ishlasa, mavjud sinflar takrorlanmaydi; o‘quvchi, baho va davomat yozuvlari saqlanadi.

Dasturchi oynasiga tegishli hisob bilan kiring. **Sinflar** bo‘limidagi **Har bir darajaga A, B, D, E qo‘shish** tugmasi faqat yetishmayotgan guruhlarni qo‘shadi va qo‘shilganlar sonini ko‘rsatadi. **Sinf qo‘shish** formasida darajani tanlang, harf yoki qisqa nom kiriting (masalan `B` yoki `5-B`); bir darajadagi takror nom saqlanmaydi. Yangi sinf uchun **Dars jadvalini avtomatik tuzish** oldindan belgilangan bo‘ladi. O‘quvchisi yoki jadval darslari bor sinfni o‘chirib bo‘lmaydi; bo‘sh sinfni o‘chirishda tasdiq so‘raladi. Ro‘yxat darajalar bo‘yicha guruhlangan va har sinfdagi o‘quvchilar hamda jadval darslari sonini ko‘rsatadi.

## O‘quv rejani to‘ldirish

Boshlang‘ich o‘quv reja faqat prototip uchun berilgan **taxminiy** ma’lumotdir. Rasmiy hisoblanmaydi va tasdiqlanmaguncha **Taxminiy — tasdiqlash kerak** belgisi ko‘rinadi. Maktabgacha va maktab ta’limi vazirligining amaldagi tayanch o‘quv rejasi PDF hujjatini ochib, fanlar va har bir bosqichning haftalik soatlarini hujjat bilan solishtiring. Maktab yoki sinf kesimidagi farqlarni ham tekshiring.

Dasturchi oynasidagi **O‘quv reja** bo‘limida fan qatorlari va 1–11-bosqich ustunlari ko‘rsatiladi. Rasmiy hujjatda o‘qitiladigan fanning bosqich katagini belgilang, ma’lum bo‘lsa haftalik soatini 0–12 oralig‘ida kiriting, so‘ng **Saqlash** tugmasini bosing. Bo‘sh soat maydoni qabul qilinadi. Saqlagandan keyin taxminiy ma’lumot belgisi yo‘qoladi; rasmiy PDF bilan tekshirish mas’uliyati foydalanuvchida qoladi. O‘qituvchi va o‘quvchi bu ma’lumotni ko‘ra oladi, lekin o‘zgartira olmaydi.

## Tayanch o‘quv reja CSV importi

`sample-curriculum.csv` so‘ralgan taxminiy fanlar ro‘yxatini beradi; `weekly_hours` qiymatlari ataylab bo‘sh qoldirilgan. Rasmiy PDF’dan olingan ma’lumotni quyidagi sarlavhali CSV shaklida tayyorlang:

```csv
grade,subject,weekly_hours
1,Ona tili,4
1,Matematika,
```

Har bir qator bitta bosqich-fan juftligini bildiradi. `grade` 1–11 oralig‘idagi butun son, `subject` fan nomi, `weekly_hours` esa bo‘sh yoki 0–12 oralig‘idagi butun son bo‘lsin. `weekly_hours` bo‘sh bo‘lsa, soat belgilanmagan hisoblanadi. Import vaqtida mavjud bo‘lmagan fan nomlari avtomatik yaratiladi.

Dasturchi oynasida **Ma’lumotlar → Tayanch o‘quv reja CSV** orqali faylni tanlang. Import yaroqli qatorlar bilan joriy o‘quv rejasini almashtiradi; xato qatorlar raqami va sababi ko‘rsatiladi hamda faqat o‘sha qator tashlab ketiladi. Importdan so‘ng kataklarni tekshirib, tasdiqlash uchun **O‘quv reja** bo‘limida saqlang. Rasmiy hujjatdan olingan ma’lumotni kiritishdan oldin zaxira nusxa saqlash tavsiya qilinadi.

## Dars jadvalini tuzish va tahrirlash

**Dars jadvali** oynasida sinflar daraja bo‘yicha guruhlangan. **Random tuzish** tanlangan sinf uchun dushanbadan shanbagacha, har kuni sozlamadagi darslar sonicha katak to‘ldiradi. Jadvalda avvaldan darslar bo‘lsa, almashtiriladigan darslar soni ko‘rsatiladi va tasdiq so‘raladi. **Hammasini random tuzish** odatda faqat jadvali bo‘sh sinflarni to‘ldiradi. Mavjud jadvallarni ham almashtirish uchun tasdiqlash oynasidagi **Qo‘lda kiritilgan jadvallarni ham almashtirish** katagini belgilang.

Generator faqat shu sinf darajasining o‘quv rejasidagi fanlardan foydalanadi. `weeklyHours` butun son qilib kiritilgan bo‘lsa, shu soat sonini ishlatadi. Bo‘sh maydon soat noma’lumligini bildiradi: noma’lum fanlarning har biri kamida bir marta olinib, qolgan kataklar ular orasida taxminan teng va tasodifiy taqsimlanadi. `0` bo‘sh qiymat emas, rejalashtirilgan nol soatni bildiradi. Rejadagi jami soat mavjud kataklardan ko‘p bo‘lsa, generator ularni mutanosib kamaytirib ogohlantiradi; kam bo‘lsa, bo‘sh kataklarni reja fanlari orasidan tasodifiy tanlab to‘ldiradi. Shu sababli jami soat yetmagan holatda nol soatli fan ham qo‘shimcha tasodifiy katakka tushishi mumkin. Natijada bir kunda bir xil fan takrorlari imkon qadar kamaytiriladi.

**Jadvalni o‘zgartirish** tugmasi kataklarni fan tanlash maydoniga va ixtiyoriy boshlanish/tugash vaqtlariga aylantiradi. **Saqlash** o‘zgarishlarni yozadi, bir kunda bir fan ikki marta tanlansa ogohlantiradi, ammo saqlashga to‘sqinlik qilmaydi. Daraja o‘quv rejasida yo‘q fan yangi jadvalga kiritilmaydi. Avval saqlangan, endi reja tashqarisida qolgan fan haqida ogohlantirish ko‘rsatiladi va uni tahrirda saqlab qolish mumkin. **Bekor qilish** tahrirni bekor qiladi. **Jadvalni tozalash** tasdiqdan keyin faqat tanlangan sinf jadvalini o‘chiradi. Jadval ostidagi jamlanma har bir fan uchun rejadagi haftalik soatni va jadvalga qo‘yilgan darslar sonini ko‘rsatadi. O‘qituvchi jadvalni faqat ko‘radi; o‘quvchi esa faqat o‘z sinfining jadvalini ko‘radi.

Brauzer konsolida `window.runTimetableTests()` buyrug‘i bilan sinf migratsiyasi, generator, haftalik soatlar, takror fanlar, sinf nomi, o‘chirish cheklovi va mavjud jadvalni almashtirish tasdig‘ini tekshirish mumkin.

## Xavfsizlik eslatmasi

**Bu prototip ma’lumot va parol xeshlarini foydalanuvchining brauzerida saqlaydi. Server tomonida autentifikatsiya va ruxsat nazorati yo‘q. Haqiqiy o‘quvchi ma’lumotlarini saqlashdan oldin server tomonida autentifikatsiya va ma’lumotlar bazasi bo‘lgan backend qo‘shilishi kerak.** Brauzer xotirasiga yoki qurilmaga kirish imkoniga ega bo‘lgan kishi ma’lumotlarni ko‘rishi yoki o‘zgartirishi mumkin.
