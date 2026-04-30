Tugas Kecerdasan Komputasional 
nama : errizal ade ragil khariurnayif 
nim : 2024 12 010 
jurusan : Teknik Informatika 

Penjelasan Code pada Kasus 4 

BAGIAN LIBARY PYTHON
import random
import matplotlib.pyplot as plt

bagian ini adalah bagian untuk meng import libary matplotlib& libary random yang berfungsi untuk membuat angka acak atau random dan membuat sebuah grafik 

BAGIAN DATA PENGELUARAN

items = [
    ("Makan", 20000, 80),
    ("Kopi", 15000, 60),
    ("Jajan", 10000, 50),
    ("Transport", 10000, 70),
    ("Internet", 5000, 40),
]
BUDGET = 50000
POP_SIZE = 30
GENERATIONS = 100
MUTATION_RATE = 0.2

bagian ini adalah bagian inisialisasi parameter dan data dalam sebuah program Algoritma Genetika untuk menyelesaikan masalah knapsack problem (masalah optimasi dalam memilih barang dengan batasan anggaran tertentu). dan bagian ini adalah bagian yang menjadi pondasi awal dalam code project ini.
 
BAGIAN INIT
def create_individual():
    return [random.randint(0,1) for _ in items]
def init_population():
    return [create_individual() for _ in range(POP_SIZE)]

Selanjutnya, bagian ini adalah Bagian ini berfungsi untuk membentuk solusi awal dalam proses algoritma genetika. Pada tahap ini, sistem membuat representasi kandidat solusi dalam bentuk individu yang tersusun dari kumpulan nilai biner (0 dan 1), di mana setiap nilai merepresentasikan keputusan pemilihan suatu item.
Fungsi create_individual() digunakan untuk membuat satu individu secara acak berdasarkan jumlah item yang tersedia. Setiap elemen dalam individu dihasilkan menggunakan nilai acak 0 atau 1, yang berarti:
1. Nilai 1 menunjukkan item dipilih
2. Nilai 0 menunjukkan item tidak dipilih
Dengan demikian, satu individu merepresentasikan kombinasi pilihan item dalam satu solusi. Selanjutnya, fungsi init_population() digunakan untuk membentuk sekumpulan individu yang disebut sebagai populasi awal. Jumlah individu dalam populasi ditentukan oleh variabel POP_SIZE, dan setiap individu dibuat menggunakan fungsi create_individual(). Bagian ini sangat bergantung pada data items yang telah didefinisikan sebelumnya, karena panjang setiap individu ditentukan oleh jumlah item yang tersedia. Selain itu, proses ini juga memanfaatkan library random untuk menghasilkan solusi secara acak.
Secara keseluruhan, bagian INIT berperan sebagai tahap awal dalam algoritma genetika yang bertujuan untuk menghasilkan populasi solusi acak yang nantinya akan dievaluasi dan dikembangkan pada tahap selanjutnya seperti seleksi, crossover, dan mutasi.

BAGIAN FITNESS
def fitness(ind):
    total_cost = 0
    total_value = 0
    for gene, (name, cost, value) in zip(ind, items):
        if gene == 1:
            total_cost += cost
            total_value += value
    if total_cost > BUDGET:
        return 0
    return total_value

lanjut dari Bagian Init, Bagian ini berfungsi untuk menentukan kualitas atau nilai dari setiap individu (solusi) yang telah dibuat pada tahap populasi awal. Dalam algoritma genetika, proses ini disebut sebagai fitness evaluation, yaitu proses penilaian seberapa baik suatu solusi terhadap tujuan yang ingin dicapai. Fungsi fitness(ind) digunakan untuk menghitung nilai fitness dari satu individu. Di dalam fungsi ini, terdapat dua variabel utama yaitu total_cost dan total_value.
1. total_cost digunakan untuk menghitung total biaya dari item yang dipilih
2. total_value digunakan untuk menghitung total nilai atau manfaat dari item tersebut
Proses perhitungan dilakukan dengan cara melakukan iterasi pada setiap gen dalam individu menggunakan zip(ind, items). Setiap gen memiliki nilai 0 atau 1 yang menunjukkan apakah suatu item dipilih atau tidak. Jika nilai gen adalah 1, maka biaya (cost) dan nilai (value) dari item tersebut akan ditambahkan ke total perhitungan. Setelah seluruh item dihitung, dilakukan pengecekan terhadap batasan yang telah ditentukan sebelumnya yaitu BUDGET. Jika total biaya melebihi batas anggaran, maka individu tersebut dianggap tidak valid dan diberikan nilai fitness sebesar 0.
Namun jika total biaya masih berada dalam batas anggaran, maka nilai fitness individu tersebut adalah total_value, yaitu total keuntungan atau nilai yang diperoleh dari kombinasi item yang dipilih.

BAGIAN SELECTION
def selection(pop):
    return max(random.sample(pop, 3), key=fitness)

selanjutnya dari Bagian Selection, Bagian ini berfungsi untuk memilih individu terbaik dari populasi berdasarkan nilai fitness yang telah dihitung sebelumnya. Proses ini merupakan salah satu tahap penting dalam algoritma genetika karena menentukan individu mana yang akan dilanjutkan ke proses berikutnya seperti crossover dan mutasi.
Fungsi selection(pop) digunakan untuk melakukan pemilihan satu individu terbaik dari sekumpulan populasi. Di dalam fungsi ini digunakan metode tournament selection, yaitu metode seleksi dengan cara mengambil beberapa individu secara acak kemudian membandingkan nilai fitness-nya.
1. random.sample(pop, 3) : Setelah itu, dari ketiga individu tersebut, dipilih satu individu yang memiliki nilai fitness tertinggi
2. max(..., key=fitness) : menggunakan ini, Artinya, individu dengan nilai fitness terbaik akan dipilih sebagai hasil seleksi.

BAGIAN CROSSOVER
def crossover(p1, p2):
    point = random.randint(1, len(p1)-1)
    return p1[:point] + p2[point:]

lanjut dari Bagian Selection, Bagian ini berfungsi untuk menghasilkan individu baru (offspring) dengan cara menggabungkan dua individu induk yang telah dipilih sebelumnya melalui proses seleksi. Dalam algoritma genetika, proses ini disebut sebagai crossover atau rekombinasi, yang bertujuan untuk menciptakan variasi solusi baru dari solusi yang sudah ada.
Fungsi crossover(p1, p2) menerima dua individu sebagai input, yaitu p1 dan p2 yang merupakan parent (induk). Kemudian, proses crossover dilakukan dengan menentukan satu titik potong (crossover point) secara acak menggunakan:
point = random.randint(1, len(p1)-1) : Titik ini menentukan posisi di mana kedua individu akan dipisahkan.
Setelah titik potong ditentukan, individu baru dibuat dengan cara menggabungkan bagian awal dari parent pertama (p1) dengan bagian akhir dari parent kedua (p2), yaitu:
p1[:point] + p2[point:] : Hasil dari proses ini adalah individu baru yang memiliki kombinasi sifat dari kedua parent, sehingga diharapkan dapat menghasilkan solusi yang lebih baik dibandingkan sebelumnya.

BAGIAN MUTATION
def mutate(ind):
    for i in range(len(ind)):
        if random.random() < MUTATION_RATE:
            ind[i] = 1 - ind[i]
    return ind

selanjutnya dari Bagian Crossover, Bagian ini berfungsi untuk melakukan perubahan acak pada individu (solusi) dalam populasi. Dalam algoritma genetika, proses ini disebut sebagai mutasi, yang bertujuan untuk menjaga keberagaman solusi agar tidak terjadi kondisi stagnan atau terlalu cepat konvergen pada satu solusi saja. Fungsi mutate(ind) digunakan untuk memodifikasi satu individu dengan cara mengecek setiap gen (elemen) di dalam individu satu per satu menggunakan perulangan.
Pada setiap gen, dilakukan pengecekan menggunakan nilai acak:
if random.random() < MUTATION_RATE: Jika kondisi tersebut terpenuhi (dalam kode ini probabilitasnya ditentukan oleh MUTATION_RATE), maka nilai gen akan dibalik.
ind[i] = 1 - ind[i] : menggunakan ini, 
Artinya:
Jika nilai awal 1 → berubah menjadi 0
Jika nilai awal 0 → berubah menjadi 1
Proses ini dilakukan untuk semua gen dalam individu, sehingga menghasilkan variasi baru dalam solusi.

BAGIAN DECODE
def decode(ind):
    chosen = []
    total_cost = 0
    total_value = 0
    for gene, (name, cost, value) in zip(ind, items):
        if gene == 1:
            chosen.append(name)
            total_cost += cost
            total_value += value
    return chosen, total_cost, total_value

lanjut dari Bagian Mutation, Bagian ini berfungsi untuk mengubah representasi solusi dari bentuk biner (individu dalam algoritma genetika) menjadi bentuk yang lebih mudah dipahami oleh manusia. Proses ini disebut sebagai decode, yaitu proses penerjemahan hasil kromosom menjadi informasi nyata berupa item yang dipilih beserta total biaya dan total nilainya. Fungsi decode(ind) menerima satu individu sebagai input, yaitu daftar yang berisi nilai 0 dan 1 yang merepresentasikan apakah suatu item dipilih atau tidak. Kemudian, fungsi ini melakukan iterasi pada setiap gen dalam individu dengan menggunakan zip(ind, items) untuk menggabungkan data gen dengan informasi item yang tersedia. Jika nilai gen adalah 1, maka item tersebut dianggap dipilih dan akan dimasukkan ke dalam daftar chosen. Selain itu, nilai cost dan value dari item tersebut akan ditambahkan ke variabel total_cost dan total_value.
Proses ini menghasilkan tiga output utama, yaitu:
1. chosen → daftar item yang terpilih
2. total_cost → total biaya dari item yang dipilih
3. total_value → total nilai atau manfaat dari item yang dipilih

BAGIAN GA
def GA():
    pop = init_population()
    best_hist = []
    print("\n===== OPTIMASI UANG JAJAN =====\n")
    for gen in range(GENERATIONS):
        pop = sorted(pop, key=fitness, reverse=True)
        best = pop[0]
        best_fit = fitness(best)
        best_hist.append(best_fit)
        if gen % 5 == 0:
            print(f"Gen {gen:3d} | Nilai kepuasan: {best_fit}")
        new_pop = pop[:2]
        while len(new_pop) < POP_SIZE:
            p1 = selection(pop)
            p2 = selection(pop)
            child = crossover(p1, p2)
            child = mutate(child)
            new_pop.append(child)
        pop = new_pop

lanjut dari Bagian Decode, Bagian ini merupakan inti dari keseluruhan program yang berfungsi untuk menjalankan proses optimasi menggunakan metode Genetic Algorithm (GA). Pada tahap ini, seluruh proses evolusi seperti seleksi, crossover, dan mutasi dijalankan secara berulang untuk mencari solusi terbaik dari permasalahan yang diberikan. Fungsi GA() diawali dengan membentuk populasi awal menggunakan init_population(), kemudian dibuat variabel best_hist untuk menyimpan perkembangan nilai terbaik dari setiap generasi. Selanjutnya, algoritma dijalankan dalam perulangan sebanyak GENERATIONS, yang merepresentasikan jumlah iterasi evolusi. 
Pada setiap generasi, langkah pertama yang dilakukan adalah mengurutkan populasi berdasarkan nilai fitness secara menurun:
pop = sorted(pop, key=fitness, reverse=True) : Individu terbaik pada generasi tersebut diambil sebagai best, kemudian nilai fitness-nya disimpan ke dalam best_hist untuk analisis perkembangan hasil.
Setiap 5 generasi, program menampilkan informasi berupa nilai kepuasan terbaik saat itu sebagai bentuk monitoring proses evolusi.
Setelah itu, dibentuk populasi baru (new_pop) yang diawali dengan mengambil dua individu terbaik (elitism) agar solusi terbaik tidak hilang:
new_pop = pop[:2] : Kemudian dilakukan proses perulangan hingga ukuran populasi kembali sesuai POP_SIZE. Pada tahap ini dilakukan:
1. Selection → memilih dua parent terbaik
2. Crossover → menggabungkan dua parent menjadi anak
3. Mutation → memberikan variasi acak pada anak
Hasil anak tersebut kemudian dimasukkan ke populasi baru. Setelah populasi baru terbentuk penuh, populasi lama digantikan dengan populasi baru dan proses ini diulang hingga jumlah generasi yang ditentukan selesai.

BAGIAN HASIL
    best = sorted(pop, key=fitness, reverse=True)[0]
    chosen, cost, value = decode(best)
    print("\n===== HASIL AKHIR =====")
    print("Pilihan:", chosen)
    print("Total biaya:", cost)
    print("Total kepuasan:", value)
    if cost <= BUDGET:
        print("Penjelasan: Pengeluaran optimal sesuai budget.")
    else:
        print("Penjelasan: Melebihi budget (tidak valid).")

selanjutnya dari Bagian Ga, Bagian ini berfungsi untuk menampilkan solusi terbaik yang diperoleh setelah seluruh proses Genetic Algorithm selesai dijalankan. Pada tahap ini, program akan mengambil individu terbaik dari populasi terakhir berdasarkan nilai fitness tertinggi.
Individu terbaik tersebut dipilih menggunakan:
best = sorted(pop, key=fitness, reverse=True)[0] : Setelah itu, individu tersebut diubah ke bentuk yang lebih mudah dipahami menggunakan fungsi decode(), sehingga diperoleh informasi berupa daftar item yang dipilih, total biaya, dan total nilai kepuasan.
Hasil tersebut kemudian ditampilkan ke pengguna dalam bentuk:
1. daftar pilihan item (chosen)
2. total biaya (cost)
3. total kepuasan (value)
Selanjutnya dilakukan pengecekan terhadap batas budget:
1. Jika total biaya ≤ BUDGET, maka solusi dianggap valid dan optimal
2. Jika melebihi budget, maka solusi dianggap tidak valid

BAGIAN VISUAL
    plt.figure()
    plt.plot(best_hist)
    plt.title("Perkembangan Kepuasan")
    plt.xlabel("Generasi")
    plt.ylabel("Fitness")
    plt.grid()
    plt.show()

RUN
GA()

dan bagian code terakhir dari project ini, Bagian ini berfungsi untuk menampilkan grafik perkembangan nilai fitness (kepuasan) dari generasi ke generasi selama proses algoritma berjalan. Data fitness terbaik setiap generasi disimpan dalam variabel best_hist, kemudian divisualisasikan menggunakan library matplotlib:
plt.plot(best_hist) : Grafik ini menunjukkan bagaimana nilai solusi terbaik berkembang seiring proses evolusi, apakah meningkat atau stagnan.
Sumbu grafik:
1. X-axis → Generasi
2. Y-axis → Nilai fitness (kepuasan)
Tujuan dari visualisasi ini adalah untuk memudahkan analisis apakah algoritma berhasil menemukan solusi yang semakin optimal dari waktu ke waktu.
Pada bagian akhir code , fungsi utama dijalankan dengan perintah:
GA() : Perintah ini akan mengeksekusi seluruh proses Genetic Algorithm mulai dari inisialisasi, perhitungan fitness, seleksi, crossover, mutasi, hingga menampilkan hasil akhir dan grafik.

OUTPUT
Gen   0 | Nilai kepuasan: 240
Gen   5 | Nilai kepuasan: 250
Gen  10 | Nilai kepuasan: 250
Gen  15 | Nilai kepuasan: 250
Gen  20 | Nilai kepuasan: 250
Gen  25 | Nilai kepuasan: 250
Gen  30 | Nilai kepuasan: 250
Gen  35 | Nilai kepuasan: 250
Gen  40 | Nilai kepuasan: 250
Gen  45 | Nilai kepuasan: 250
Gen  50 | Nilai kepuasan: 250
Gen  55 | Nilai kepuasan: 250
Gen  60 | Nilai kepuasan: 250
Gen  65 | Nilai kepuasan: 250
Gen  70 | Nilai kepuasan: 250
Gen  75 | Nilai kepuasan: 250
Gen  80 | Nilai kepuasan: 250
Gen  85 | Nilai kepuasan: 250
Gen  90 | Nilai kepuasan: 250
Gen  95 | Nilai kepuasan: 250

HASIL AKHIR 
Pilihan: ['Makan', 'Kopi', 'Transport', 'Internet']
Total biaya: 50000
Total kepuasan: 250
Penjelasan: Pengeluaran optimal sesuai budget.

Hasil output menunjukkan proses optimasi menggunakan Genetic Algorithm dalam memilih kombinasi pengeluaran terbaik agar mendapatkan nilai kepuasan maksimal tanpa melebihi batas budget. Pada bagian awal, program menampilkan perkembangan nilai kepuasan terbaik (fitness) pada setiap generasi. Terlihat bahwa pada generasi ke-0 nilai kepuasan sebesar 240, kemudian meningkat menjadi 250 pada generasi ke-5 dan seterusnya stabil hingga generasi ke-95. Hal ini menunjukkan bahwa algoritma berhasil menemukan solusi optimal cukup cepat, yaitu pada generasi awal, dan kemudian mempertahankan solusi terbaik tersebut tanpa perubahan signifikan hingga akhir proses. Kondisi ini menandakan bahwa algoritma sudah mencapai konvergensi, yaitu tidak adanya peningkatan solusi lebih lanjut karena sudah ditemukan nilai terbaik.

GAMBAR GRAFIK
Grafik ini menunjukkan perkembangan nilai fitness (kepuasan) dari solusi terbaik pada setiap generasi selama proses algoritma genetika berjalan. Sumbu horizontal (X) merepresentasikan jumlah generasi, sedangkan sumbu vertikal (Y) menunjukkan nilai fitness atau tingkat kepuasan dari solusi terbaik pada setiap generasi. Dari grafik terlihat bahwa pada awal proses (generasi ke-0), nilai kepuasan berada di angka sekitar 240. Kemudian pada generasi awal berikutnya, terjadi peningkatan nilai fitness hingga mencapai 250.
Setelah mencapai nilai 250, grafik menunjukkan garis yang stabil dan tidak mengalami peningkatan lagi hingga generasi ke-100. Hal ini menandakan bahwa algoritma telah mencapai kondisi konvergensi, yaitu keadaan di mana solusi terbaik sudah ditemukan dan tidak ada lagi perbaikan signifikan pada generasi berikutnya.
