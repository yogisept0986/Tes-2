<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fitur Pencarian ala Shopee</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        html, body {
            width: 100%;
            overflow-x: hidden;
            margin: 0;
            padding: 0;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Roboto', Arial, sans-serif;
        }

        body {
            background-color: #f2f2f2;
            color: #333;
        }
        
        /* PERBAIKAN: Tambahan style untuk desktop view - memastikan header responsif */
        .desktop-view .header {
            padding: 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden; /* Mencegah overflow content */
        }
        
        .desktop-view .search-container {
            max-width: 800px;
            margin: 0 auto;
            width: 100%;
        }
        
        .desktop-view .search-expanded {
            display: flex;
            justify-content: center;
        }
        
        .desktop-view .search-expanded .search-container {
            width: 100%;
            max-width: 800px;
        }

        /* Header dan Navigasi */
        .header {
            background-color: #ee4d2d;
            padding: 10px 15px;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            z-index: 1000;
            transition: all 0.3s ease;
            box-sizing: border-box; /* PERBAIKAN: Memastikan padding tidak menambah lebar total */
        }

        .search-container {
            display: flex;
            align-items: center;
            position: relative;
            max-width: 800px;
            margin: 0 auto;
            width: 100%;
            box-sizing: border-box; /* PERBAIKAN: Memastikan padding tidak menambah lebar */
        }

        .search-input {
            flex-grow: 1;
            padding: 10px 40px 10px 15px;
            border: 2px solid #ee4d2d;
            border-radius: 8px;
            font-size: 14px;
            outline: none;
            color: #333;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }
    .header .search-input {
    flex-grow: 1;
    padding: 10px 60px 10px 15px;
    border: 2px solid #ee4d2d;
    border-radius: 8px; /* Tetap pertahankan border radius */
    font-size: 14px;
    outline: none;
    color: #333;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    width: calc(100% - 30px); /* Potong bagian kanan input sebesar 30px */
    margin-right: 80px; /* Geser ke kanan untuk mempertahankan posisi ikon pencarian */
}
    
    /* Styling untuk ikon keranjang dan chat */
.header-icons {
    display: flex;
    align-items: center;
    position: absolute;
    right: 8px; /* Jarak dari tepi kanan search-container */
    top: 50%;
    padding-top: 5px;
    transform: translateY(-50%);
    z-index: 2; /* Pastikan berada di atas elemen lain */
}
  
.cart-icon, .chat-icon {
    position: relative;
    color: transparent;
    font-size: 22px;
    cursor: pointer;
}

/* Styling khusus untuk ikon keranjang dengan garis yang lebih tipis */
.cart-icon {
    -webkit-text-stroke: 1.3px white; /* Menipis kan garis untuk keranjang */
}

/* Styling khusus untuk ikon chat dengan garis yang tetap tebal */
.chat-icon {
    -webkit-text-stroke: 2px white; /* Tetap mempertahankan ketebalan garis chat */
    margin-left: 25px; /* Jarak ke kanan tetap sama */
}

/* Tambahkan jarak antara ikon keranjang dan chat */
.chat-icon {
    margin-left: 20px;
}

/* Badge notifikasi dengan background putih dan angka oranye */
.cart-badge, .chat-badge {
    position: absolute;
    top: -8px;
    right: -8px;
    background-color: white;
    color: #ee4d2d; /* Warna oranye Shopee */
    font-size: 11px;
    font-weight: bold;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1;
}

/* Styling untuk titik-titik di dalam ikon chat */
.chat-icon .chat-dots {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 60%;
    height: 30%;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.chat-icon .chat-dots .dot {
    width: 3px;
    height: 3px;
    background-color: white;
    border-radius: 50%;
}


@media (max-width: 480px) {
    .cart-icon, .chat-icon {
        font-size: 20px;
    }
    
    .cart-icon {
        -webkit-text-stroke: 0.8px white; /* Garis lebih tipis untuk keranjang pada layar kecil */
    }
    
    .chat-icon {
        -webkit-text-stroke: 1.2px white; /* Garis untuk chat pada layar kecil */
        margin-left: 20px;
    }
    
    .chat-icon {
        margin-left: 15px;
    }
    
    .cart-badge, .chat-badge {
        width: 16px;
        height: 16px;
        font-size: 10px;
        -webkit-text-stroke: 0.4px #ee4d2d; /* Garis lebih tipis pada layar kecil */
    }
    
    .chat-icon .chat-dots .dot {
        width: 2px;
        height: 2px;
    }
}

/* Pastikan container tidak overflow */
.header .search-container {
    display: flex;
    align-items: center;
    position: relative;
    max-width: 800px;
    margin: 0 auto;
    width: 100%;
    box-sizing: border-box;
    overflow: hidden; /* Mencegah overflow */
}
.header .search-icon {
    position: absolute;
    right: 95px; /* Sesuaikan dengan margin-right dari input + sedikit tambahan */
    color: #ee4d2d;
    cursor: pointer;
    font-size: 16px;
}
        /* Placeholder warna oranye */
        .search-input::placeholder {
            color: #ee4d2d;
            opacity: 1;
        }

        /* Placeholder animasi - PERBAIKAN: Animasi berfungsi di kedua tampilan dan tidak hilang pada interaksi lain */
        .search-placeholder {
            position: absolute;
            left: 15px;
            top: 50%;
            transform: translateY(-50%);
            color: #ee4d2d;
            pointer-events: none;
            font-size: 14px;
            transition: opacity 0.3s;
            display: flex !important; /* Memastikan selalu tampil kecuali saat focus/ada nilai */
            flex-direction: column;
            width: calc(100% - 60px);
            overflow: hidden;
            height: 20px;
            z-index: 1; /* Memastikan placeholder tetap terlihat */
        }

        .placeholder-static {
            white-space: nowrap;
            position: absolute;
            top: 0;
            left: 0;
            opacity: 1;
            animation: staticPlaceholderAnimation 12s infinite;
        }

        .placeholder-dynamic {
            position: relative;
            width: 100%;
            overflow: hidden;
            height: 20px;
        }

        .placeholder-text {
            position: absolute;
            width: 100%;
            display: flex;
            align-items: center;
            opacity: 0;
            top: 0;
            animation: placeholderAnimation 12s infinite;
            white-space: nowrap;
        }

        @keyframes staticPlaceholderAnimation {
            0%, 4% {
                opacity: 1;
            }
            8%, 96% {
                opacity: 0;
            }
            100% {
                opacity: 0;
            }
        }

        @keyframes placeholderAnimation {
            0% {
                opacity: 0;
                transform: translateY(20px);
            }
            8%, 25% {
                opacity: 1;
                transform: translateY(0);
            }
            33% {
                opacity: 0;
                transform: translateY(-20px);
            }
            100% {
                opacity: 0;
                transform: translateY(-20px);
            }
        }

        .placeholder-text:nth-child(2) {
            animation-delay: 3s;
        }

        .placeholder-text:nth-child(3) {
            animation-delay: 6s;
        }

        .placeholder-text:nth-child(4) {
            animation-delay: 9s;
        }

        /* Sembunyikan placeholder ketika input focus atau memiliki nilai */
        .search-input:focus + .search-placeholder,
        .search-input:not(:placeholder-shown) + .search-placeholder,
        .hide-placeholder + .search-placeholder {
            opacity: 0 !important;
        }

        /* PERBAIKAN: Tambahan class untuk menyembunyikan placeholder */
        .force-hide-placeholder {
            opacity: 0 !important;
        }

        .search-icon {
    position: absolute;
    right: 15px; /* Tetap di posisi kanan */
    color: #ee4d2d;
    cursor: pointer;
    font-size: 16px;
    z-index: 3; /* Pastikan masih di atas header-icons */
}

        /* PERBAIKAN: Style untuk ikon silang sesuai gaya Shopee - DIPERBESAR */
  .clear-search-icon {
            position: absolute;
            right: 50px; /* PERBAIKAN: Posisi lebih ke kiri */
            top: 50%;
            transform: translateY(-50%);
            width: 20px; /* PERBAIKAN: Ukuran icon tetap sama */
            height: 20px; /* PERBAIKAN: Ukuran icon tetap sama */
            background-color: #ccc;
            border-radius: 50%;
            cursor: pointer;
            display: none; /* Sembunyikan awalnya */
            z-index: 3; /* Pastikan di atas elemen lain */
            align-items: center;
            justify-content: center;
            padding: 0; /* PERBAIKAN: Hapus padding yang mungkin mengganggu klik */
            border: none; /* PERBAIKAN: Hapus border yang mungkin mengganggu klik */
            -webkit-tap-highlight-color: transparent; /* PERBAIKAN: Hapus highlight saat di-tap di mobile */
            touch-action: manipulation; /* PERBAIKAN: Perbaiki perilaku touch di mobile */
        }
        
        /* NEW: Tambahkan elemen pseudo untuk memperluas area klik tanpa mengubah ukuran visual */
        .clear-search-icon::before {
            content: '';
            position: absolute;
            width: 40px; /* Area klik yang diperbesar */
            height: 40px; /* Area klik yang diperbesar */
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        
        
        .clear-search-icon i {
            font-size: 12px; /* PERBAIKAN: Ukuran ikon diperbesar */
            color: white;
            line-height: 1;
            pointer-events: none; /* PERBAIKAN: Mencegah ikon sendiri menangkap event */
        }
        
        /* PERBAIKAN: Tambahkan efek hover untuk memberikan indikasi interaktif */
        .clear-search-icon:hover {
            background-color: #aaa;
        }
        
        /* PERBAIKAN: Tambahkan efek tekan untuk umpan balik visual */
        .clear-search-icon:active {
            background-color: #999;
            transform: translateY(-50%) scale(0.95);
        }
        
        /* Pada expanded search, geser posisinya sesuai dengan gaya Shopee */
        .search-expanded .clear-search-icon {
            right: 68px; /* PERBAIKAN: Posisi lebih ke kiri dari ikon pencarian di expanded mode */
        }
/* Icon pencarian di luar kolom pada expanded view */
        .expanded-search-icon {
            background-color: #ee4d2d;
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-left: 10px;
            cursor: pointer;
        }

        /* Overlay area */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            bottom: 0px;
            width: 100%;
            height: 100%;
            background-color: white;
            z-index: 999;
            display: none;
            overflow-y: auto;
            box-sizing: border-box; /* PERBAIKAN: Memastikan padding tidak menambah lebar */
        }

        /* Search expanded mode */
        .search-expanded {
            background-color: white;
            padding: 10px 15px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            z-index: 1001;
            box-sizing: border-box;
        }

        .back-btn {
            margin-right: 10px;
            color: #ee4d2d;
            cursor: pointer;
            display: flex;
            align-items: center;
        }

        /* Icon panah dibuat lebih panjang */
        .back-btn i {
            font-size: 20px;
            transform: scaleX(1.3);
        }

        /* Suggestions */
        .suggestions-container {
            background-color: white;
            width: 100%;
            max-width: 800px;
            margin: 0 auto;
            margin-top: 0px; /* PERBAIKAN: Mengoptimalkan space untuk filter tabs */
            border-radius: 3px;
            overflow: hidden;
            display: none;
            transition: all 0.3s ease;
            box-sizing: border-box;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            padding: 0; /* PERBAIKAN: Menghilangkan padding untuk mencegah overflow */
            /* FIX: Memastikan tidak ada gap antara search dan suggestions */
            position: absolute;
            top: 0px;
            left: 0;
            right: 0;
            transform: translateY(-20px); /* Tambahkan transform untuk menggeser ke atas */
        }

        .text-suggestions {
            padding: 10px 0px 10px 0px;
            border-bottom: 1px solid #f2f2f2;
            display: block; /* PERBAIKAN BAGIAN 1: Tampilkan text suggestions */
        }

        /* PERBAIKAN: Perbarui display suggestion menjadi hanya 2 kata */
        .suggestion-item {
            padding: 10px 15px;
            cursor: pointer;
            display: flex;
            align-items: center;
        }

        .suggestion-item:hover {
            background-color: #f9f9f9;
        }

        .suggestion-icon {
            color: #999;
            margin-right: 10px;
            font-size: 14px;
            width: 15px;
            display: inline-block;
        }

        /* PERBAIKAN: Style untuk tombol Lihat Lainnya agar dapat berpindah posisi dan tidak terpotong */
        .see-more-container {
            padding: 5px 0;
            text-align: center;
            transition: all 0.2s ease;
            position: relative; /* Menambahkan posisi relatif */
            z-index: 2; /* Meningkatkan z-index untuk memastikan tetap terlihat */
            background-color: white; /* Pastikan latar belakang putih */
            margin-bottom: 15px; /* PERBAIKAN: Tambahkan margin untuk memberikan jarak */
            display: block; /* PERBAIKAN BAGIAN 1: Tampilkan tombol lihat lainnya */
        }
        
        .see-more-btn {
            background-color: transparent;
            color: #ee4d2d;
            border: none;
            padding: 10px;
            font-size: 14px;
            cursor: pointer;
            border-radius: 4px;
            font-weight: 500;
            width: 100%;
            max-width: 300px;
            margin: 0 auto;
            text-align: center;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background-color 0.2s ease;
        }
        
        .see-more-btn:hover {
    background-color: transparent;
        }        
        
        .see-more-btn i {
            margin-right: 8px;
            color: #ee4d2d;
            font-size: 14px;
        }
  /* PERBAIKAN: Style untuk additional suggestions dengan animasi slide down */
        .additional-suggestions {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease;
            border-bottom: 0px solid #f2f2f2;
            background-color: white;
            position: relative;
            z-index: 1;
            margin-bottom: 15px; /* PERBAIKAN: Tambahkan margin untuk memberikan jarak */
            display: block; /* PERBAIKAN BAGIAN 1: Tampilkan additional suggestions */
        }
        
        .additional-suggestions.open {
            max-height: 350px; /* PERBAIKAN: Tinggi maksimal saat terbuka */
            border-bottom: 1px solid #f2f2f2;
            padding-bottom: 20px; /* PERBAIKAN: Tambahkan padding bawah */
        }
        
        .additional-suggestions .suggestion-item {
            padding: 10px 15px;
            cursor: pointer;
            display: flex;
            align-items: center;
        }
        .additional-suggestions .suggestion-icon {
    color: #ee4d2d;
      }
        .additional-suggestions .suggestion-item:hover {
            background-color: #f9f9f;
        }
/* Product suggestions */
        .product-suggestions {
            padding: 10px;
            width: 100%;
            box-sizing: border-box;
            margin-top: 10px; /* PERBAIKAN: Tambahkan margin untuk memberikan jarak */
        }

        .product-title {
            font-size: 14px;
            color: #666;
            margin-bottom: 15px;
            padding: 0 5px;
            display: block; /* PERBAIKAN BAGIAN 1: Tampilkan product title */
        }

        /* PERBAIKAN: Grid produk standar */
        .product-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            width: 100%;
            box-sizing: border-box;
            border: 2px solid #eee;
            border-radius: 5px;
            margin-top: 20px;
            padding: 10px 10px 25px 10px; /* JARAK DIOPTIMALKAN: Mengurangi padding atas agar tidak terlalu longgar */
            transform: translateY(0px); 
        }

        /* PERBAIKAN: Card produk standar (full layout) */
        .product-card {
            background-color: white;
            border-radius: 3px;
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0,0,0,0.15);
            cursor: pointer;
            transform: translateY(10px); 
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }

        .product-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }

        /* PERBAIKAN: Card produk simpel (tampilan pencarian awal) */
        .simple-product-card {
            background-color: white;
            border-radius: 3px;
            transform: translateY(10px);
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0,0,0,0.15);
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            text-align: center;
        }

        .simple-product-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }

        .product-img {
            width: 100%;
            height: 150px;
            object-fit: cover;
            display: block;
        }

        .product-info {
            padding: 10px;
        }

        /* PERBAIKAN TAMPILAN CARD: Style untuk icon shopee mall */
        .shopee-mall-icon {
            display: inline-flex;
            align-items: center;
            background-color: #ee4d2d;
            color: white;
            font-size: 9px;
            font-weight: bold;
            padding: 1px 3px;
            border-radius: 2px;
            margin-right: 4px;
            vertical-align: middle;
        }

        /* PERBAIKAN 2: Nama produk standar dengan 2 baris */
        .product-name {
            font-size: 13px;
            color: #333;
            margin-bottom: 5px;
            height: 36px;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            line-height: 1.4;
        }

        /* PERBAIKAN TAMPILAN CARD: Rating dan terjual */
        .product-rating-sold {
            display: flex;
            align-items: center;
            font-size: 12px;
            color: #666;
            margin-bottom: 5px;
        }

        .product-rating {
            color: #ee4d2d;
            margin-right: 8px;
            display: flex;
            align-items: center;
        }

        .product-rating i {
            font-size: 11px;
            margin-right: 2px;
        }

        .product-sold {
            color: #666;
            position: relative;
            padding-left: 8px;
        }

        .product-sold:before {
            content: '';
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            height: 10px;
            width: 1px;
            background-color: #ccc;
        }

        /* PERBAIKAN TAMPILAN CARD: Pengiriman */
        .product-shipping {
            display: flex;
            align-items: center;
            font-size: 11px;
            color: #666;
            margin-bottom: 5px;
        }

        .product-shipping i {
            color: #00bfa5;
            margin-right: 4px;
            font-size: 12px;
        }

        /* PERBAIKAN: Nama produk untuk tampilan simpel (3 kata) */
        .simple-product-name {
            font-size: 13px;
            color: #333;
            padding: 10px 5px;
            text-align: center;
            height: auto;
            max-height: 40px;
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
        }

        .product-price {
            font-size: 14px;
            color: #ee4d2d;
            font-weight: bold;
        }

        /* PERBAIKAN 3: Perbaikan container harga agar tidak terpotong */
        .product-price-container {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            margin-bottom: 5px;
        }

        .product-price-original {
            font-size: 12px;
            color: #999;
            text-decoration: line-through;
            margin-left: 5px;
        }

        .product-discount {
            font-size: 10px;
            color: #ee4d2d;
            background-color: #ffeee8;
            padding: 1px 4px;
            border-radius: 2px;
            margin-left: 5px;
        }

        /* PERBAIKAN 5: Warna COD menjadi oranye seperti Shopee */
        .cod-icon {
            display: inline-flex;
            position: absolute;
            bottom: 10px;
            right: 10px;
            background-color: #ee4d2d;
            color: white;
            font-size: 9px;
            font-weight: bold;
            padding: 2px 4px;
            border-radius: 2px;
            align-items: center;
        }

        .cod-icon i {
            font-size: 10px;
            margin-right: 2px;
        }
/* Content area */
        .content-area {
            padding: 70px 15px 20px;
            max-width: 1200px;
            margin: 0 auto;
        }

        /* Keyword Suggestions Animation */
        .keyword-suggestions {
            margin: 20px auto;
            max-width: 800px;
            height: 40px;
            overflow: hidden;
            position: relative;
            display: none; /* PERBAIKAN: Sembunyikan keyword suggestions pada tampilan desktop */
        }

        .keyword-suggestions-wrapper {
            position: relative;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .keyword-static {
            color: #333;
            font-size: 14px;
            margin-right: 5px;
        }

        .keyword-dynamic {
            position: relative;
            width: 200px;
            height: 20px;
            overflow: hidden;
        }

        .keyword {
            position: absolute;
            width: 100%;
            color: #ee4d2d;
            font-size: 14px;
            display: flex;
            align-items: center;
            opacity: 0;
            animation: keywordAnimation 10s infinite;
            white-space: nowrap;
        }

        /* Animasi keyword agar loop tanpa kembali */
        @keyframes keywordAnimation {
            0% {
                opacity: 0;
                transform: translateY(20px);
            }
            5%, 20% {
                opacity: 1;
                transform: translateY(0);
            }
            25% {
                opacity: 0;
                transform: translateY(-20px);
            }
            100% {
                opacity: 0;
                transform: translateY(-20px);
            }
        }

        .keyword:nth-child(2) {
            animation-delay: 2.5s;
        }

        .keyword:nth-child(3) {
            animation-delay: 5s;
        }

        .keyword:nth-child(4) {
            animation-delay: 7.5s;
        }
/* Styling untuk product header - PERBAIKAN: Membuat mobile-friendly untuk nama produk */
        .product-header {
            display: none;
            background-color: white;
            padding: 16px;
            border-bottom: 1px solid #f2f2f2;
            margin-bottom: 5px;
        }

        .product-header .product-info {
            display: flex;
            align-items: center;
        }

        .product-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            overflow: hidden;
            margin-right: 15px;
            border: 1px solid #eee;
            flex-shrink: 0; /* PERBAIKAN: Mencegah avatar mengecil */
        }

        .product-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .product-details {
            flex: 1;
            cursor: pointer;
            min-width: 0; /* PERBAIKAN: Memastikan flex item bisa menyusut di bawah min-content width */
            overflow: hidden; /* PERBAIKAN: Mencegah overflow */
        }

        .product-name {
            font-size: 16px;
            font-weight: 500;
            margin-bottom: 2px;
            white-space: nowrap; /* PERBAIKAN: Memastikan nama produk dalam satu baris */
            overflow: hidden; /* PERBAIKAN: Menyembunyikan teks yang overflow */
            text-overflow: ellipsis; /* PERBAIKAN: Menambahkan elipsis (…) */
        }

        .product-category {
            font-size: 12px;
            color: #666;
            margin-bottom: 5px;
            white-space: nowrap; /* PERBAIKAN: Mencegah wrap */
            overflow: hidden; /* PERBAIKAN: Menyembunyikan overflow */
            text-overflow: ellipsis; /* PERBAIKAN: Menambahkan elipsis */
        }

        .product-stats {
            font-size: 12px;
            color: #666;
            white-space: nowrap; /* PERBAIKAN: Mencegah wrap */
            overflow: hidden; /* PERBAIKAN: Menyembunyikan overflow */
        }

        .product-stats span {
            margin-right: 10px;
        }

        .product-action {
            color: #ee4d2d;
            font-size: 14px;
            display: flex;
            align-items: center;
            cursor: pointer;
            margin-left: 10px; /* PERBAIKAN: Tambahkan margin untuk menjaga jarak */
            flex-shrink: 0; /* PERBAIKAN: Mencegah tombol menyusut */
        }

        .product-action i {
            font-size: 16px;
            margin-left: 5px;
        }

        /* PERBAIKAN: Style untuk tab filter agar tampil dengan benar */
        .filter-tabs {
            display: none;
            background-color: white;
            width: 100%;
            border-bottom: 1px solid #f2f2f2;
            overflow-x: auto;
            white-space: nowrap;
            padding: 0;
            margin: 0;
            margin-bottom: 5px; /* JARAK DIOPTIMALKAN: Mengurangi margin bawah tab filter */
            -webkit-overflow-scrolling: touch;
            position: fixed;
            top: 60px; /* Posisi tepat di bawah search bar */
            left: 0;
            right: 0;
            z-index: 1002;
            height: 45px; /* JARAK DIOPTIMALKAN: Tetapkan tinggi untuk tab filter */
        }
        
        .filter-tabs-inner {
            display: inline-flex;
            padding: 0 10px;
            padding-left: 25px; 
        }
        
        .filter-tab {
            padding: 12px 15px;
            font-size: 14px;
            color: #666;
            cursor: pointer;
            position: relative;
            transition: color 0.2s ease;
        }
        
        .filter-tab.active {
            color: #ee4d2d;
            font-weight: 500;
        }
        
        .filter-tab.active:after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 15px;
            right: 15px;
            height: 2px;
            background-color: #ee4d2d;
        }
        
        .filter-tab-price {
            display: flex;
            align-items: center;
        }
        
        .filter-tab-price i {
            margin-left: 5px;
            font-size: 12px;
        }
        
        /*  Atur Responsive styles - PERBAIKAN UNTUK DESKTOP */
		
		@media (max-width: 480px) {
			.cart-icon, .chat-icon {
				font-size: 20px;
			}
			
			
			.chat-icon {
				-webkit-text-stroke: 0.8px white; /* Garis untuk chat pada layar kecil */
				margin-left: 15px;
			}
			
			
			.cart-badge, .chat-badge {
				width: 16px;
				height: 16px;
				font-size: 10px;
				-webkit-text-stroke: 0.4px #ee4d2d; /* Garis lebih tipis pada layar kecil */
			}
			
			.chat-icon .chat-dots .dot {
				width: 2px;
				height: 2px;
			}
		}
		
		@media (max-width: 576px) {
            .not-found-container {
                padding: 20px 15px 30px;
                margin: 10px 0;
            }
            
            .not-found-button-container {
                max-width: 100%;
            }
            
            .not-found-message {
                max-width: 95%;
            }
            
            .keyword-suggestions-popup {
                max-width: 100%;
                border-radius: 12px 12px 0 0;
            }
            
            .product-grid, .products-grid {
                grid-template-columns: repeat(2, 1fr);
                gap: 8px;
                padding-top: 5px; /* JARAK DIOPTIMALKAN: Mengurangi padding di mobile juga */
            }
            
            .product-card, .simple-product-card {
                box-shadow: 0 1px 5px rgba(0,0,0,0.1);
            }
            
            .product-img {
                height: 130px;
            }
            
            .not-found-icon {
                width: 100px;
                height: 100px;
            }
            
            .not-found-button {
                padding: 10px 12px;
                font-size: 13px;
            }
            
            /* PERBAIKAN: CSS untuk product header pada mobile */
            .product-header {
                padding: 12px;
            }
            
            .product-avatar {
                width: 40px;
                height: 40px;
                margin-right: 10px;
            }
            
            .product-action {
                font-size: 12px;
            }
            
            .product-action i {
                font-size: 14px;
            }
            
            /* PERBAIKAN: Memastikan keyword suggestions tampil di mobile */
            .keyword-suggestions {
                display: block;
            }
            
            /* FIX: Memastikan suggestions-container tetap posisi absolute dan di bawah search bar */
            .suggestions-container {
                position: absolute;
                top: 60px;
                margin-top: 0;
            }
        }
  @media (max-width: 767px) {
    .product-grid {
        width: 100vw;
        position: relative;
        left: 50%;
        right: 50%;
        margin-left: -50vw;
        margin-right: -50vw;
        border-left: none;
        border-right: none;
        border-radius: 0;
        padding-left: 10px;
        padding-right: 10px;
    }
}
		
        @media (min-width: 768px) {
		
		  .cart-icon, .chat-icon {
				font-size: 20px;
			}
			.cart-icon {
				-webkit-text-stroke: 0.8px white; /* Menipis kan garis untuk keranjang */
			}
			
			
			.chat-icon {
				-webkit-text-stroke: 0.8px white; /* Garis untuk chat pada layar kecil */
				margin-left: 15px;
			}
			
			
			.cart-badge, .chat-badge {
				width: 16px;
				height: 16px;
				font-size: 10px;
				-webkit-text-stroke: 0.4px #ee4d2d; /* Garis lebih tipis pada layar kecil */
			}
			
			.chat-icon .chat-dots .dot {
				width: 2px;
				height: 2px;
			}
		
          .filter-tabs {
            display: flex;
            justify-content: center;
            padding: 0;
            width: 100%;
          }

          .filter-tabs-inner {
            max-width: 800px;
            width: 100%;
            display: flex;
            justify-content: space-between;
            padding: 0;
            margin: 0 auto; /* Tambahkan margin auto untuk memastikan posisi tengah */
          }

          .filter-tab {
            flex: 1;
            text-align: center;
            white-space: nowrap;
            padding: 12px 20px;
          }
          
          .filter-tab.filter-tab-price {
            display: flex;
            justify-content: center;
            align-items: center;
          }

          .filter-tab-price i {
            margin-left: 5px;
            margin-right: -5px; /* Kompensasi margin agar teks lebih terpusat */
          }
          
          .product-grid {
            grid-template-columns: repeat(3, 1fr);
            width: 100%;
            box-sizing: border-box;
          }
            
          .search-container {
            max-width: 800px;
            margin: 0 auto;
            width: 100%;
            box-sizing: border-box;
          }
            
          .header, .search-expanded {
            padding: 15px;
            width: 100%;
            box-sizing: border-box;
          }
            
          .suggestions-container {
            max-width: 800px;
            margin: 0 auto;
            margin-top: 105px; /* JARAK DIOPTIMALKAN: Mengoptimalkan jarak untuk desktop */
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            border-radius: 8px;
            width: 100%;
            box-sizing: border-box;
            padding: 0;
            /* FIX: Memastikan container tetap posisi absolut di tampilan desktop */
            position: absolute;
            top: 60px;
            transform: translateY(0px);
          }
            
          .content-area {
            max-width: 1200px;
            margin: 0 auto;
            padding: 80px 20px 30px;
            width: 100%;
            box-sizing: border-box;
          }
		.product-card, .simple-product-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
          }
            
          .product-img {
            height: 180px;
          }
            
          .product-name, .simple-product-name {
            font-size: 14px;
          }
            
          .product-suggestions, .text-suggestions, .additional-suggestions {
            width: 100%;
            box-sizing: border-box;
          }
            
          /* PERBAIKAN: Tampilkan keyword suggestions pada tampilan mobile saja */
          .keyword-suggestions {
            display: block;
          }
		  .keyword-suggestions-popup {
                max-width: 800px;
            }
        }
		
        
		
		@media (min-width: 1024px) {
            .product-grid {
                grid-template-columns: repeat(4, 1fr);
                gap: 15px;
            }
            
            .search-container {
                max-width: 800px;
            }
            
            .suggestions-container {
                max-width: 800px;
                /* FIX: Memastikan container tetap posisi absolut di tampilan lebar */
                position: absolute;
                top: 60px;
            }
            
            .product-img {
                height: 200px;
            }
        }
        
       
        
  /* Tambahan styles untuk suggestions-container */
        /* FIX: Gaya untuk menghilangkan gap antara search dan suggestions */
        .search-expanded + .suggestions-container {
            margin-top: 0 !important;
            top: 60px !important;
        }
        
        /* JARAK DIOPTIMALKAN: Menambahkan margin lebih kecil di bawah filter tabs */
        .filter-tabs + .suggestions-container {
            margin-top: 8px; /* Jarak yang lebih minimal namun masih mencegah terpotong */
        }
        
        /* JARAK DIOPTIMALKAN: Atur ulang posisi dan margin saat filter tabs tampil */
        .search-expanded + .filter-tabs + .suggestions-container {
            margin-top: 0 !important;
            top: 97px !important; /* JARAK DIOPTIMALKAN: Posisi 60px (search bar) + 37px (filter tabs height) */
            padding-top: 5px; /* JARAK DIOPTIMALKAN: Mengurangi padding atas pada suggestions container */
        }

        /* Tampilan Hasil Tidak Ditemukan yang diperbarui */
        .not-found-container {
            display: none;
            text-align: center;
            padding: 30px 20px 40px;
            margin: 0;
            background-color: #fff;
            width: 100%;
            position: relative;
        }

        .not-found-icon {
            margin: 0 auto 20px;
            width: 120px;
            height: 120px;
            opacity: 0.5;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .search-document-icon {
            position: relative;
            width: 100%;
            height: 100%;
        }

        .document-base {
            position: absolute;
            width: 70px;
            height: 85px;
            background-color: #ddd;
            border-radius: 5px;
            top: 20px;
            left: 25px;
        }

        .document-fold {
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: #eee;
            top: 20px;
            right: 25px;
            clip-path: polygon(0 0, 100% 100%, 100% 0);
        }

        .document-lines {
            position: absolute;
            width: 40px;
            height: 4px;
            background-color: #ccc;
            top: 45px;
            left: 40px;
        }

        .document-lines:before {
            content: '';
            position: absolute;
            width: 30px;
            height: 4px;
            background-color: #ccc;
            top: 12px;
            left: 0;
        }

        .document-lines:after {
            content: '';
            position: absolute;
            width: 20px;
            height: 4px;
            background-color: #ccc;
            top: 24px;
            left: 0;
        }

        .magnifying-glass {
            position: absolute;
            width: 40px;
            height: 40px;
            border: 6px solid #ccc;
            border-radius: 50%;
            top: 10px;
            right: 10px;
        }
        
        .magnifying-glass:after {
            content: '';
            position: absolute;
            width: 6px;
            height: 20px;
            background-color: #ccc;
            transform: rotate(-45deg);
            bottom: -15px;
            right: -5px;
            border-radius: 10px;
        }

        .not-found-title {
            font-size: 18px;
            font-weight: 500;
            color: #333;
            margin-bottom: 10px;
        }

        .not-found-message {
            font-size: 14px;
            color: #888;
            margin-bottom: 25px;
        }

        .not-found-button-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
            max-width: 400px;
            margin: 0 auto;
        }

        .not-found-button {
            border-radius: 4px;
            padding: 12px 16px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            border: none;
            transition: all 0.2s ease;
        }

        .not-found-button.primary {
            background-color: #ee4d2d;
            color: white;
        }

        .not-found-button.primary:hover {
            background-color: #e04224;
        }

        .not-found-button.secondary {
            background-color: white;
            color: #ee4d2d;
            border: 1px solid #ee4d2d;
        }

        .not-found-button.secondary:hover {
            background-color: rgba(238, 77, 45, 0.05);
        }

        /* PERBAIKAN: Ganti ke keyword tag sugesti lagi (bukan popup produk) */
        .keyword-suggestions-popup {
            display: none;
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background-color: white;
            padding: 20px;
            border-top-left-radius: 16px;
            border-top-right-radius: 16px;
            box-shadow: 0 -4px 10px rgba(0,0,0,0.1);
            z-index: 1000;
            max-height: 70vh;
            overflow-y: auto;
            animation: slideUp 0.3s ease;
            max-width: 800px;
            margin: 0 auto;
        }

        @keyframes slideUp {
            from {
                transform: translateY(100%);
            }
            to {
                transform: translateY(0);
            }
        }

        .keyword-suggestions-title {
            font-size: 16px;
            font-weight: 500;
            color: #333;
            margin-bottom: 15px;
            text-align: center;
        }

        /* Keyword suggestion tags */
        .keyword-suggestions-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            width: 100%;
        }

        .keyword-suggestion-tag {
            background-color: #f5f5f5;
            border-radius: 20px;
            padding: 10px 15px;
            font-size: 13px;
            color: #333;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .keyword-suggestion-tag:hover {
            background-color: #ffe8e3;
            color: #ee4d2d;
        }

        /* Products Container */
        .products-container {
            display: none;
            padding: 15px;
            background-color: #fff;
            width: 100%;
        }

        .products-title {
            font-size: 14px;
            color: #666;
            margin-bottom: 15px;
            padding: 0 5px;
        }

        .products-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            width: 100%;
            box-sizing: border-box;
        }

        /* Overlay untuk popup */
        .suggestions-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0,0,0,0.5);
            z-index: 999;
        }

        /* Helper classes */
        .hide {
            display: none !important;
        }

        .show {
            display: block !important;
        }

        /* Show suggestions */
        .show-suggestions .keyword-suggestions-popup,
        .show-suggestions .suggestions-overlay {
            display: block;
        }

       
        /* Tambahan: Pastikan keyword suggestions tidak terlihat di tampilan pencarian expanded */
        .search-expanded ~ .keyword-suggestions {
            display: none !important;
        }
        
        /* Perbaikan bug: Paksa sembunyikan keyword suggestions di berbagai kondisi */
        .overlay[style*="display: block"] ~ .keyword-suggestions,
        #searchOverlay[style*="display: block"] ~ .keyword-suggestions,
        .search-expanded ~ .keyword-suggestions,
        .keyword-suggestions {
            display: none !important;
            visibility: hidden !important;
            opacity: 0 !important;
            pointer-events: none !important;
            max-height: 0 !important;
            overflow: hidden !important;
        }

        /* Fokus spesifik pada keyword_suggestions */
        #keywordSuggestions {
            display: none !important;
        }
        
        /* Perbaikan khusus untuk placeholder oranye di tampilan 2 */
        .search-expanded .search-placeholder,
        .search-expanded .search-input:not(:focus) + .search-placeholder {
            display: none !important;
            opacity: 0 !important;
            visibility: hidden !important;
            pointer-events: none !important;
        }

        /* SOLUSI: Style untuk menyembunyikan filter tabs secara paksa saat peringatan ditampilkan */
        .not-found-mode .filter-tabs,
        .other-products-mode .filter-tabs {
            display: none !important;
            visibility: hidden !important;
            opacity: 0 !important;
            height: 0 !important;
            overflow: hidden !important;
            position: absolute !important;
            top: -9999px !important;
            left: -9999px !important;
            pointer-events: none !important;
            z-index: -1 !important;
            max-height: 0 !important;
            max-width: 0 !important;
            margin: 0 !important;
            padding: 0 !important;
            border: none !important;
            clip: rect(0, 0, 0, 0) !important;
            clip-path: inset(100%) !important;
        }
  /* Styling untuk "Hapus riwayat pencarian" */
.clear-history {
    text-align: center;
    padding: 10px;
    margin-bottom: 5px;
    cursor: pointer;
    color: #999;
    font-size: 13px;
    border-bottom: 1px solid #f2f2f2;
}

.clear-history:hover {
    color: #ee4d2d;
}

/* Styling untuk "Sedang Trend" */
.trending-title {
   margin-top: 15px;
    color: #333;
    font-weight: 500;
    padding: 12px 15px 5px;
    font-size: 13px;
    display: flex;
    align-items: center;
}

.trending-title .trend-icon {
    margin-left: 5px;
    font-size: 13px;
    color: #ee4d2d;
}
  
  /* Tampilan Pill dengan Badge untuk Sedang Trend */
.trend-pill {
    display: inline-flex;
    align-items: center;
    background-color: #fff4f2;
    color: #ee4d2d;
    font-size: 16px;
    font-weight: 500;
    padding: 6px 15px; /* Padding diperbesar sedikit */
    border-radius: 15px;
    margin: 20px 0 20px 15px;
    max-width: 160px;
}

.trend-pill-badge {
    background-color: #ee4d2d;
    color: white;
    font-size: 10px;
    border-radius: 50%;
    width: 18px;
    height: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-left: 8px;
}
  
    </style>
</head>
<body>
<!-- Header normal dengan search bar -->
    
<header class="header" id="mainHeader">
    <div class="search-container">
        <!-- PERBAIKAN: Tidak menampilkan ikon silang di tampilan pertama -->
        <input type="text" class="search-input" id="searchInput" placeholder=" ">
        <div class="search-placeholder" id="searchPlaceholder">
            <div class="placeholder-dynamic">
                <!-- PERBAIKAN: Keyword hanya 2 kata -->
                <div class="placeholder-text">Handphone Samsung</div>
                <div class="placeholder-text">Sepatu Pria</div>
                <div class="placeholder-text">Tas Wanita</div>
                <div class="placeholder-text">Promo Elektronik</div>
            </div>
        </div>
        <div class="search-icon">
            <i class="fa fa-search"></i>
        </div>
        
        <!-- Tambahkan ikon keranjang dan chat di sini -->
        <div class="header-icons">
            <!-- Ikon keranjang dengan badge -->
            <div class="cart-icon">
                <i class="fa fa-shopping-cart"></i>
                <div class="cart-badge">5</div>
            </div>
            
            <!-- Ikon chat dengan titik-titik -->
            <div class="chat-icon">
                <i class="fa fa-comment"></i>
                <div class="chat-badge">3</div>
                <div class="chat-dots">
                    <span class="dot"></span>
                    <span class="dot"></span>
                    <span class="dot"></span>
                </div>
            </div>
        </div>
    </div>
</header>
    <!-- Keyword suggestions vertical -->
    <div class="keyword-suggestions" id="keywordSuggestions">
        <div class="keyword-suggestions-wrapper">
            
            <div class="keyword-dynamic">
              
            </div>
        </div>
    </div>

    <!-- Overlay saat search aktif -->
    <div class="overlay" id="searchOverlay">
        <!-- Search bar expanded mode -->
        <div class="search-expanded">
            <div class="search-container">
                <span class="back-btn" id="backBtn"><i class="fa fa-arrow-left"></i></span>
                <input type="text" class="search-input" id="expandedSearchInput" placeholder=" ">
                <div class="search-placeholder" id="expandedSearchPlaceholder">
                    <div class="placeholder-dynamic">
                        <!-- PERBAIKAN: Keyword hanya 2 kata -->
                        <div class="placeholder-text">Handphone Samsung</div>
                        <div class="placeholder-text">Sepatu Pria</div>
                        <div class="placeholder-text">Tas Wanita</div>
                        <div class="placeholder-text">Promo Elektronik</div>
                    </div>
                </div>
                <!-- PERBAIKAN: Ikon silang hanya ditampilkan di tampilan kedua (expanded) -->
                <!-- PERBAIKAN: Mengubah ikon silang menjadi button untuk meningkatkan responsivitas -->
                <button class="clear-search-icon" id="expandedClearSearchIcon" aria-label="Hapus pencarian">
                    <i class="fa fa-times"></i>
                </button>
                <!-- Icon pencarian dipindahkan ke luar input box -->
                <div class="expanded-search-icon" id="expandedSearchIcon">
                    <i class="fa fa-search"></i>
                </div>
            </div>
        </div>

        <!-- PERBAIKAN: Tab filter di bawah kolom pencarian dengan posisi fixed -->
        <div class="filter-tabs" id="filterTabs">
            <div class="filter-tabs-inner">
                <div class="filter-tab active" data-filter="terkait">Terkait</div>
                <div class="filter-tab" data-filter="terlaris">Terlaris</div>
                <div class="filter-tab" data-filter="terbaru">Terbaru</div>
                <div class="filter-tab filter-tab-price" data-filter="harga">
                    Harga <i class="fa fa-sort" id="priceSort"></i>
                </div>
            </div>
        </div>
<!-- Suggestions container -->
        <div class="suggestions-container" id="suggestionsContainer">
           <!-- Tambahkan "Hapus riwayat pencarian" di atas text suggestions -->
<div class="clear-history" id="clearHistoryBtn">Hapus riwayat pencarian</div>

<!-- Text suggestions -->
<div class="text-suggestions">
    <!-- PERBAIKAN: Suggestion item hanya 2 kata -->
    <div class="suggestion-item" data-text="Smartphone Android">
        <span class="suggestion-icon"><i class="fa fa-history"></i></span>
        <span>Smartphone Android</span>
    </div>
    <div class="suggestion-item" data-text="Sepatu Sneakers">
        <span class="suggestion-icon"><i class="fa fa-history"></i></span>
        <span>Sepatu Sneakers</span>
    </div>
    <div class="suggestion-item" data-text="Tas Selempang">
        <span class="suggestion-icon"><i class="fa fa-history"></i></span>
        <span>Tas Selempang</span>
    </div>
</div>
            
            <!-- PERBAIKAN: Tombol Lihat Lainnya di atas additional suggestions -->
            <div class="see-more-container">
                <button class="see-more-btn" id="seeMoreBtn">
                    <i class="fa fa-plus-circle"></i>
                    <span>Lihat Lainnya</span>
                </button>
            </div>
            
            <!-- PERBAIKAN: Additional suggestions dengan animasi slide down - ISI DENGAN NAMA PRODUK -->
<div class="additional-suggestions" id="additionalSuggestions">
   <!-- Tampilan Pill dengan Badge untuk Sedang Trend -->
<div class="trend-pill">
    Sedang Trend
    <div class="trend-pill-badge" id="trendCount">0</div>
</div>
  
    <!-- PERBAIKAN: Suggestion item hanya 2 kata -->
    <div class="suggestion-item" data-text="Headphone Bluetooth">
        <span class="suggestion-icon"><i class="fa fa-fire"></i></span>
        <span>Headphone Bluetooth</span>
    </div>
                <div class="suggestion-item" data-text="Keyboard Gaming">
                    <span class="suggestion-icon"><i class="fa fa-fire"></i></span>
                    <span>Keyboard Gaming</span>
                </div>
                <div class="suggestion-item" data-text="Power Bank">
                    <span class="suggestion-icon"><i class="fa fa-fire"></i></span>
                    <span>Power Bank</span>
                </div>
                <div class="suggestion-item" data-text="Smart TV">
                    <span class="suggestion-icon"><i class="fa fa-tag"></i></span>
                    <span>Smart TV</span>
                </div>
            </div>
<!-- Product header yang akan muncul di atas card peringatan - PERBAIKAN: Mencegah overflow nama produk -->
            <div class="product-header" id="productHeader" style="display: none;">
                <div class="product-info">
                    <div class="product-avatar">
                        <img src="/api/placeholder/100/100" alt="Product image">
                    </div>
                    <div class="product-details" id="productDetails">
                        <div class="product-name">Smartphone 128GB</div>
                        <div class="product-category">Elektronik</div>
                        <div class="product-stats">
                            <span>Rp 2.999.000</span>
                            <span>★ 4.8</span>
                        </div>
                    </div>
                    <div class="product-action" id="productAction">
                        Produk Lainnya <i class="fa fa-chevron-right"></i>
                    </div>
                </div>
            </div>
            
            <div class="not-found-container" id="notFoundContainer">
                <div class="not-found-icon">
                    <div class="search-document-icon">
                        <div class="document-base"></div>
                        <div class="document-fold"></div>
                        <div class="document-lines"></div>
                        <div class="magnifying-glass"></div>
                    </div>
                </div>
                <div class="not-found-title">Hasil tidak ditemukan</div>
                <div class="not-found-message">Mohon coba kata kunci yang lain atau yang lebih umum.</div>
                <div class="not-found-button-container">
                    <button class="not-found-button primary" id="tryAnotherKeywordBtn">Coba kata kunci lain</button>
                    <button class="not-found-button secondary" id="trySuggestionBtn">Coba produk lainnya</button>
                </div>
            </div>
            <!-- Tambahkan tampilan produk lainnya -->
            <div class="products-container" id="productsContainer">
                <div class="products-title">Produk Lainnya</div>
                <div class="products-grid" id="otherProductsGrid">
                    <!-- Product cards akan diisi secara dinamis -->
                </div>
            </div>

             <!-- Product suggestions -->
            <div class="product-suggestions">
                <div class="product-title">Produk Populer</div>
                <div class="product-grid" id="productGrid">
                    <!-- Product cards will be inserted here via JavaScript -->
                </div>
            </div>
            
            <!-- PERBAIKAN: Gunakan keyword tags dengan nama produk, bukan card produk -->
            <div class="keyword-suggestions-popup" id="keywordSuggestionsPopup">
                <div class="keyword-suggestions-title">Produk Populer</div>
                <div class="keyword-suggestions-grid">
<div class="keyword-suggestion-tag" data-product="Smartphone Android">Smartphone Android</div>
                    <div class="keyword-suggestion-tag" data-product="Sepatu Sneakers">Sepatu Sneakers</div>
                    <div class="keyword-suggestion-tag" data-product="Tas Selempang">Tas Selempang</div>
                    <div class="keyword-suggestion-tag" data-product="Headphone Bluetooth">Headphone Bluetooth</div>
                    <div class="keyword-suggestion-tag" data-product="Keyboard Gaming">Keyboard Gaming</div>
                    <div class="keyword-suggestion-tag" data-product="Power Bank">Power Bank</div>
                    <div class="keyword-suggestion-tag" data-product="Smart TV">Smart TV</div>
                    <div class="keyword-suggestion-tag" data-product="Robot Vacuum">Robot Vacuum</div>
                </div>
            </div>

            <!-- Overlay untuk popup -->
            <div class="suggestions-overlay" id="suggestionsOverlay"></div>
        </div>
    </div>

    <!-- Content area -->
    <div class="content-area" id="contentArea">
        <!-- Content area kosong -->
    </div>
<script>
        // Fungsi untuk memastikan responsivitas desktop
        function adjustDesktopLayout() {
            // Hanya jalankan untuk desktop (>= 768px)
            if (window.innerWidth >= 768) {
                // Pastikan container suggestion tidak melebihi viewport
                if (document.querySelector('.suggestions-container')) {
                    const container = document.querySelector('.suggestions-container');
                    
                    // Reset ukuran untuk menghindari overflow
                    container.style.width = '100%';
                    container.style.maxWidth = '800px';
                    container.style.boxSizing = 'border-box';
                    
                    // Pastikan produk grid juga tidak overflow
                    if (document.querySelector('.product-grid')) {
                        document.querySelector('.product-grid').style.width = '100%';
                        document.querySelector('.product-grid').style.boxSizing = 'border-box';
                    }
                }
                
                // PERBAIKAN: Memastikan header tidak melebihi lebar halaman
                const header = document.getElementById('mainHeader');
                if (header) {
                    header.style.width = '100%';
                    header.style.boxSizing = 'border-box';
                }
                
                // PERBAIKAN: Sembunyikan keyword suggestions pada tampilan desktop
                if (document.getElementById('keywordSuggestions')) {
                    document.getElementById('keywordSuggestions').style.display = 'none';
                }
            } else {
                // PERBAIKAN: Tampilkan keyword suggestions pada tampilan mobile
                if (document.getElementById('keywordSuggestions')) {
                    document.getElementById('keywordSuggestions').style.display = 'block';
                }
            }
        }
        
        // Jalankan saat halaman dimuat dan saat resize window
        window.addEventListener('load', adjustDesktopLayout);
        window.addEventListener('resize', adjustDesktopLayout);
    
        // Elemen DOM
        const mainHeader = document.getElementById('mainHeader');
        const clearHistoryBtn = document.getElementById('clearHistoryBtn');
        const searchInput = document.getElementById('searchInput');
        const expandedSearchInput = document.getElementById('expandedSearchInput');
        const searchOverlay = document.getElementById('searchOverlay');
        const suggestionsContainer = document.getElementById('suggestionsContainer');
        const backBtn = document.getElementById('backBtn');
        const contentArea = document.getElementById('contentArea');
        const productGrid = document.getElementById('productGrid');
        const suggestionItems = document.querySelectorAll('.suggestion-item');
        const keywordSuggestions = document.getElementById('keywordSuggestions');
        const searchPlaceholder = document.getElementById('searchPlaceholder');
        const expandedSearchPlaceholder = document.getElementById('expandedSearchPlaceholder');
        const expandedSearchIcon = document.getElementById('expandedSearchIcon');
        const seeMoreBtn = document.getElementById('seeMoreBtn');
        const additionalSuggestions = document.getElementById('additionalSuggestions');
        const notFoundContainer = document.getElementById('notFoundContainer');
        const productsContainer = document.getElementById('productsContainer');
        const otherProductsGrid = document.getElementById('otherProductsGrid');
        const keywordSuggestionsPopup = document.getElementById('keywordSuggestionsPopup');
        const suggestionsOverlay = document.getElementById('suggestionsOverlay');
        const tryAnotherKeywordBtn = document.getElementById('tryAnotherKeywordBtn');
        const trySuggestionBtn = document.getElementById('trySuggestionBtn');
        const textSuggestions = document.querySelector('.text-suggestions');
        
        // Fungsi untuk menghapus riwayat pencarian
function clearSearchHistory() {
    const textSuggestionsContainer = document.querySelector('.text-suggestions');
    
    // Hapus semua suggestion item
    while (textSuggestionsContainer.firstChild) {
        textSuggestionsContainer.removeChild(textSuggestionsContainer.firstChild);
    }
    
    // Tambahkan pesan bahwa riwayat telah dihapus
    const emptyMessage = document.createElement('div');
    emptyMessage.className = 'suggestion-item empty-history';
    emptyMessage.innerHTML = '<span style="color: #999; font-style: italic; padding: 10px 15px;">Tidak ada riwayat pencarian</span>';
    textSuggestionsContainer.appendChild(emptyMessage);
    
    console.log("Riwayat pencarian dihapus");
}
  // Event listener untuk tombol hapus riwayat pencarian
if (clearHistoryBtn) {
    clearHistoryBtn.addEventListener('click', function(e) {
        e.preventDefault();
        e.stopPropagation();
        clearSearchHistory();
    });
}
        // PERBAIKAN: Elemen filter tabs
        const filterTabs = document.getElementById('filterTabs');
        const priceSort = document.getElementById('priceSort');
        
        // PERBAIKAN: Elemen ikon silang (hanya di expanded search)
        const expandedClearSearchIcon = document.getElementById('expandedClearSearchIcon');
        let productActionVisible = true;
  // Data produk sampel
        const sampleProducts = [
            {
                id: 1,
                name: "Smartphone Android Samsung",
                price: "Rp 2.999.000",
                originalPrice: "Rp 3.499.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "15%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "handphone",
                shortName: "Samsung Galaxy", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: true, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.9, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 125, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Instan", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: true // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
            {
                id: 2,
                name: "Sepatu Sneakers Pria",
                price: "Rp 299.000",
                originalPrice: "Rp 399.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "25%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "sepatu pria",
                shortName: "Sneakers Pria", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: false, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.8, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 215, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Reguler", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: true // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
            {
                id: 3,
                name: "Tas Selempang Wanita",
                price: "Rp 179.000",
                originalPrice: "Rp 229.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "22%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "tas wanita",
                shortName: "Tas Selempang", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: true, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.7, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 98, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Instan", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: false // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
            {
                id: 4,
                name: "Headphone Bluetooth Wireless",
                price: "Rp 549.000",
                originalPrice: "Rp 699.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "21%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "elektronik",
                shortName: "Headphone Bluetooth", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: true, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.6, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 85, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Next Day", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: true // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
            {
                id: 5,
                name: "Keyboard Gaming Mechanical",
                price: "Rp 459.000",
                originalPrice: "Rp 559.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "18%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "elektronik",
                shortName: "Keyboard Gaming", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: false, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.9, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 64, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Reguler", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: false // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
            {
                id: 6,
                name: "Power Bank Quick Charge",
                price: "Rp 229.000",
                originalPrice: "Rp 299.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "23%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "handphone",
                shortName: "Power Bank", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: true, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.7, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 178, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Instan", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: true // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
            {
                id: 7,
                name: "Smart TV Android UHD",
                price: "Rp 7.499.000",
                originalPrice: "Rp 8.999.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "17%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "elektronik",
                shortName: "Smart TV", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: true, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.8, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 25, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Next Day", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: false // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
     {
                id: 8,
                name: "Tas Selempang mahal ",
                price: "Rp 169.000",
                originalPrice: "Rp 229.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "22%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "tas wanita",
                shortName: "Tas Selempang", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: true, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.7, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 9, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Instan", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: false // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            },
            {
                id: 9,
                name: "Robot Vacuum Cleaner Smart",
                price: "Rp 2.499.000",
                originalPrice: "Rp 2.999.000", // PERBAIKAN BAGIAN 2: Tambahkan harga asli
                discount: "17%", // PERBAIKAN BAGIAN 2: Tambahkan diskon
                image: "/api/placeholder/200/200",
                category: "elektronik",
                shortName: "Robot Vacuum", // PERBAIKAN: Menambahkan nama pendek untuk tampilan simple
                isMall: false, // PERBAIKAN TAMPILAN CARD: Flag untuk Shopee Mall
                rating: 4.5, // PERBAIKAN TAMPILAN CARD: Rating produk
                sold: 42, // PERBAIKAN TAMPILAN CARD: Jumlah terjual
                shipping: "Pengiriman Reguler", // PERBAIKAN TAMPILAN CARD: Info pengiriman
                cod: true // PERBAIKAN BAGIAN 2: Tambahkan flag untuk COD
            }
        ];

        // PERBAIKAN 4: Variabel untuk filter
        let currentFilter = 'terkait';
        let priceSortDirection = 'asc'; // 'asc' untuk termurah, 'desc' untuk termahal
        
        // PERBAIKAN: Flag untuk menandai apakah hasil pencarian sedang ditampilkan
        let isSearchResultShown = false;
        
        // PERBAIKAN: Fungsi untuk memastikan tab filter terlihat
        function forceShowFilterTabs() {
            if (!filterTabs) return;
            
            // Pastikan overlay tidak dalam mode khusus yang menyembunyikan filter tabs
            if (searchOverlay.classList.contains('not-found-mode') || 
                searchOverlay.classList.contains('other-products-mode')) {
                return; // Jangan tampilkan filter tabs dalam mode-mode khusus
            }
            
            // Pastikan tab filter terlihat
            filterTabs.style.display = 'block';
            filterTabs.style.visibility = 'visible';
            filterTabs.style.opacity = '1';
            
            console.log("Tab filter dipaksa tampil:", filterTabs.style.display);
        }

        // PERBAIKAN: Fungsi untuk memastikan tab filter tersembunyi
        function forceHideFilterTabs() {
            if (!filterTabs) return;
            
            // Pastikan tab filter tersembunyi
            filterTabs.style.display = 'none';
            filterTabs.style.visibility = 'hidden';
            filterTabs.style.opacity = '0';
            
            console.log("Tab filter dipaksa sembunyi:", filterTabs.style.display);
        }
        
        // PERBAIKAN: Fungsi untuk mengaktifkan mode hasil pencarian tidak ditemukan
        function activateNotFoundMode() {
            // Tambahkan class khusus ke searchOverlay
            searchOverlay.classList.add('not-found-mode');
            
            // Pastikan tab filter tidak muncul
            forceHideFilterTabs();
            
            console.log("Mode 'hasil tidak ditemukan' diaktifkan");
        }
        
        // PERBAIKAN: Fungsi untuk mengaktifkan mode produk lainnya
        function activateOtherProductsMode() {
            // Tambahkan class khusus ke searchOverlay
            searchOverlay.classList.add('other-products-mode');
            
            // Pastikan tab filter tidak muncul
            forceHideFilterTabs();
            
            console.log("Mode 'produk lainnya' diaktifkan");
        }
        
        // PERBAIKAN: Fungsi untuk menonaktifkan mode-mode khusus
        function deactivateSpecialModes() {
            // Hapus semua class mode khusus
            searchOverlay.classList.remove('not-found-mode', 'other-products-mode');
            
            console.log("Mode khusus dinonaktifkan");
        }
        
        // Fungsi untuk menampilkan produk lainnya
        function showOtherProducts() {
            // PERBAIKAN: Aktifkan mode produk lainnya
            activateOtherProductsMode();
            
            // Sembunyikan tampilan tidak ditemukan
            notFoundContainer.style.display = 'none';
            
            // Tampilkan container produk lainnya
            productsContainer.style.display = 'block';
            
            // Sembunyikan tombol "Produk Lainnya"
            const productAction = document.querySelector('.product-action');
            if (productAction) {
                productAction.style.display = 'none';
                productActionVisible = false;
            }
            
            // Isi grid produk lainnya
            otherProductsGrid.innerHTML = '';
            
            // PERBAIKAN: Gunakan tampilan card standar untuk produk lainnya
            sampleProducts.forEach(product => {
                const productCard = document.createElement('div');
                productCard.className = 'product-card'; // Tampilan standar
                productCard.style.position = 'relative'; // Tambahkan posisi relatif untuk icon COD
                
                // PERBAIKAN: Tambahkan icon mall, rating, jumlah terjual, dan info pengiriman dengan harga coret dan diskon
                productCard.innerHTML = `
                    <img src="${product.image}" alt="${product.name}" class="product-img">
                    <div class="product-info">
                        <div class="product-name">
                            ${product.isMall ? '<span class="shopee-mall-icon">Mall</span>' : ''}
                            ${product.name}
                        </div>
                        <div class="product-rating-sold">
                            <div class="product-rating"><i class="fa fa-star"></i>${product.rating}</div>
                            <div class="product-sold">Terjual ${product.sold}</div>
                        </div>
                        <div class="product-shipping"><i class="fa fa-truck"></i>${product.shipping}</div>
                        <div class="product-price-container">
                            <div class="product-price">${product.price}</div>
                            <div class="product-price-original">${product.originalPrice}</div>
                            <div class="product-discount">-${product.discount}</div>
                        </div>
                    </div>
                    ${product.cod ? '<div class="cod-icon"><i class="fa fa-money-bill"></i>COD</div>' : ''}
                `;
                
                // Tambahkan event click untuk mengarahkan ke Facebook
                productCard.addEventListener('click', function() {
                    window.location.href = 'https://www.facebook.com';
                });
                otherProductsGrid.appendChild(productCard);
            });
        }
 // PERBAIKAN: Fungsi untuk render produk simpel (tampilan kedua)
function renderSimpleProducts() {
    productGrid.innerHTML = '';
    
    // PERBAIKAN: Memastikan grid produk ditampilkan
    productGrid.style.display = 'grid';
    
    // PERBAIKAN: Sembunyikan tampilan produk lainnya
    productsContainer.style.display = 'none';
    
    // PERBAIKAN: Sembunyikan tampilan peringatan jika ada
    notFoundContainer.style.display = 'none';
    
    // PERBAIKAN: Reset product header
    document.getElementById('productHeader').style.display = 'none';
    
    // Gunakan tampilan card simpel
    sampleProducts.forEach(product => {
        const productCard = document.createElement('div');
        productCard.className = 'simple-product-card'; // Tampilan simpel
        productCard.innerHTML = `
            <img src="${product.image}" alt="${product.shortName}" class="product-img">
            <div class="simple-product-name">${product.shortName}</div>
        `;
        // Tambahkan event click untuk mengarahkan ke Facebook
        productCard.addEventListener('click', function() {
            window.location.href = 'https://www.facebook.com';
        });
        productGrid.appendChild(productCard);
    });
}
        
        // PERBAIKAN 4: Fungsi untuk mengaplikasikan filter pada produk
        function applyProductFilter(products) {
            let result = [...products];
            
            // Implementasi filter sesuai dengan tab yang aktif
            switch (currentFilter) {
                case 'terlaris':
                    result.sort((a, b) => b.sold - a.sold);
                    break;
                case 'terbaru':
                    // Dalam contoh ini, kita anggap id produk terbaru memiliki id terbesar
                    result.sort((a, b) => b.id - a.id);
                    break;
                case 'harga':
                    // Sort berdasarkan harga (hilangkan format dan konversi ke angka)
                    result.sort((a, b) => {
                        const priceA = parseInt(a.price.replace(/\D/g, ''));
                        const priceB = parseInt(b.price.replace(/\D/g, ''));
                        return priceSortDirection === 'asc' ? priceA - priceB : priceB - priceA;
                    });
                    break;
                default: // 'terkait' atau default
                    // Tidak perlu sorting khusus
                    break;
            }
            
            return result;
        }
        
        // PERBAIKAN: Render product cards standar - untuk hasil pencarian
        function renderStandardProducts(filterText = '', fromButtonClick = false) {
            // PERBAIKAN: Nonaktifkan mode khusus
            deactivateSpecialModes();
            // PERBAIKAN 1: Tandai bahwa hasil pencarian sedang ditampilkan jika dari button click
if (fromButtonClick) {
    isSearchResultShown = true;
    // Sembunyikan judul "Produk Populer" saat menampilkan hasil pencarian
    document.querySelector('.product-title').style.display = 'none';
    // Sembunyikan sugesti keyword dan elemen terkait
    textSuggestions.style.display = 'none';
    document.querySelector('.see-more-container').style.display = 'none';
    document.querySelector('.additional-suggestions').style.display = 'none';
    
    // Sembunyikan "Hapus riwayat pencarian" dan "Sedang Trend"
if (clearHistoryBtn) clearHistoryBtn.style.display = 'none';
const trendPill = document.querySelector('.trend-pill');
if (trendPill) trendPill.style.display = 'none';
    
    // PERBAIKAN: Tampilkan tab filter dengan memaksa style terlihat
    forceShowFilterTabs();
}
             else {
                // Saat bukan dari button click (misalnya saat input), tetap tampilkan sugesti
                document.querySelector('.product-title').style.display = 'block';
                textSuggestions.style.display = 'block';
                document.querySelector('.see-more-container').style.display = 'block';
                document.querySelector('.additional-suggestions').style.display = 'block';
            }
            
            productGrid.innerHTML = '';
            
            let filteredProducts = sampleProducts;
            
            // Filter produk berdasarkan teks pencarian jika ada
            if (filterText) {
                filteredProducts = sampleProducts.filter(product => 
                    product.name.toLowerCase().includes(filterText.toLowerCase()) ||
                    product.category.toLowerCase().includes(filterText.toLowerCase())
                );
            }
            
            // Cek apakah hasil pencarian kosong
            if (filteredProducts.length === 0 && filterText !== '' && fromButtonClick) {
                // PERBAIKAN: Aktifkan mode khusus untuk hasil tidak ditemukan
                activateNotFoundMode();
                
                // Tampilkan product header
                document.getElementById('productHeader').style.display = 'block';
                
                // Tampilkan pesan tidak ditemukan
                notFoundContainer.style.display = 'block';
                productGrid.style.display = 'none';
                document.querySelector('.product-title').style.display = 'none';
                
                // Sembunyikan sugesti keyword lainnya
                textSuggestions.style.display = 'none';
                document.querySelector('.see-more-container').style.display = 'none';
                document.querySelector('.additional-suggestions').style.display = 'none';
                
                // Reset tampilan produk lainnya
                productsContainer.style.display = 'none';
                
                // Pastikan tombol "Produk Lainnya" terlihat
                const productAction = document.querySelector('.product-action');
                if (productAction) {
                    productAction.style.display = 'flex';
                    productActionVisible = true;
                }
            } else {
                // Sembunyikan product header
                document.getElementById('productHeader').style.display = 'none';
                
                // Sembunyikan pesan tidak ditemukan dan tampilkan produk
                notFoundContainer.style.display = 'none';
                productsContainer.style.display = 'none';
                productGrid.style.display = 'grid';
                
                // PERBAIKAN 4: Aplikasikan filter pada produk
                if (fromButtonClick) {
                    // Filter tabs harus ditampilkan jika dari button click
                    forceShowFilterTabs();
                    
                    // Aplikasikan filter
                    filteredProducts = applyProductFilter(filteredProducts);
                    
                    // Paksa tampilkan tab dengan delay
                    setTimeout(forceShowFilterTabs, 100);
                } else {
                    forceHideFilterTabs();
                }
                
                // PERBAIKAN: Render produk dengan tampilan standar
                filteredProducts.forEach(product => {
                    const productCard = document.createElement('div');
                    productCard.className = 'product-card'; // Tampilan standar
                    productCard.style.position = 'relative'; // Tambahkan posisi relatif untuk icon COD
                    
                    // PERBAIKAN: Tambahkan icon mall, rating, jumlah terjual, dan info pengiriman dengan harga coret dan diskon
                    productCard.innerHTML = `
                        <img src="${product.image}" alt="${product.name}" class="product-img">
                        <div class="product-info">
                            <div class="product-name">
                                ${product.isMall ? '<span class="shopee-mall-icon">Mall</span>' : ''}
                                ${product.name}
                            </div>
                            <div class="product-rating-sold">
                                <div class="product-rating"><i class="fa fa-star"></i>${product.rating}</div>
                                <div class="product-sold">Terjual ${product.sold}</div>
                            </div>
                            <div class="product-shipping"><i class="fa fa-truck"></i>${product.shipping}</div>
                            <div class="product-price-container">
                                <div class="product-price">${product.price}</div>
                                <div class="product-price-original">${product.originalPrice}</div>
                                <div class="product-discount">-${product.discount}</div>
                            </div>
                        </div>
                        ${product.cod ? '<div class="cod-icon"><i class="fa fa-money-bill"></i>COD</div>' : ''}
                    `;
                    
                    // Tambahkan event click untuk mengarahkan ke Facebook
                    productCard.addEventListener('click', function() {
                        window.location.href = 'https://www.facebook.com';
                    });
                    productGrid.appendChild(productCard);
                });
            }
        }
// PERBAIKAN: Fungsi untuk menampilkan/menyembunyikan ikon silang yang lebih responsif
        function toggleClearIcon(input) {
            if (input && input.value && input.value.trim().length > 0) {
                expandedClearSearchIcon.style.display = 'flex';
            } else {
                expandedClearSearchIcon.style.display = 'none';
            }
        }
        
       // PERBAIKAN: Fungsi untuk membersihkan input pencarian
function clearSearchInput() {
    expandedSearchInput.value = '';
    expandedSearchInput.focus();
    expandedClearSearchIcon.style.display = 'none';
    expandedSearchPlaceholder.classList.remove('force-hide-placeholder');
    
  // PERBAIKAN: Tampilkan kembali elemen "Hapus riwayat pencarian" dan "Sedang Trend"
if (clearHistoryBtn) clearHistoryBtn.style.display = 'block';
const trendPill = document.querySelector('.trend-pill');
if (trendPill) trendPill.style.display = 'flex';
  
    // PERBAIKAN: Reset isSearchResultShown saat membersihkan input
    isSearchResultShown = false;
    
    // PERBAIKAN: Nonaktifkan mode khusus
    deactivateSpecialModes();
    
    // PERBAIKAN: Sembunyikan tab filter saat membersihkan pencarian
    forceHideFilterTabs();
    
    // PERBAIKAN: Sembunyikan tampilan produk lainnya
    productsContainer.style.display = 'none';
    
    // PERBAIKAN: Sembunyikan tampilan peringatan jika ada
    notFoundContainer.style.display = 'none';
    
    // PERBAIKAN: Reset product header
    document.getElementById('productHeader').style.display = 'none';
    
    // PERBAIKAN: Reset tampilan sugesti
    textSuggestions.style.display = 'block';
    
    // PERBAIKAN: Reset tombol Lihat Lainnya dan additional suggestions
    document.querySelector('.see-more-container').style.display = 'block';
    document.querySelector('.additional-suggestions').style.display = 'block';
    document.querySelector('.additional-suggestions').classList.remove('open');
    document.querySelector('.product-title').style.display = 'block';
    
    // PERBAIKAN: Kembalikan tombol Lihat Lainnya ke posisi awal jika berada di dalam additional suggestions
    if (document.querySelector('.see-more-container').parentNode === additionalSuggestions) {
        textSuggestions.parentNode.insertBefore(
            document.querySelector('.see-more-container'), 
            additionalSuggestions
        );
        seeMoreBtn.innerHTML = '<i class="fa fa-plus-circle"></i><span>Lihat Lainnya</span>';
    }
    
    // PERBAIKAN: Tampilkan tampilan simple setelah menghapus pencarian
    renderSimpleProducts();
}
        
        // PERBAIKAN: Event listener untuk ikon silang yang lebih responsif
        // Menggunakan click dan mousedown untuk memastikan respons cepat
        expandedClearSearchIcon.addEventListener('click', function(e) {
            e.preventDefault(); // Mencegah default browser behavior
            e.stopPropagation(); // Mencegah event menyebar ke elemen lain
            clearSearchInput();
        });
        
        // PERBAIKAN: Tambahan event listener untuk responsivitas lebih baik pada mobile
        expandedClearSearchIcon.addEventListener('touchstart', function(e) {
            e.preventDefault(); // Mencegah default browser behavior
            e.stopPropagation(); // Mencegah event menyebar ke elemen lain
            clearSearchInput();
        }, { passive: false });
        
        // PERBAIKAN: Mencegah focus loss saat tombol diklik
        expandedClearSearchIcon.addEventListener('mousedown', function(e) {
            e.preventDefault(); // Mencegah default browser behavior yang mungkin menghilangkan fokus dari input
        });
        
        // Tampilkan mode pencarian
        function showSearchMode() {
            searchOverlay.style.display = 'block';
            mainHeader.style.display = 'none';
            contentArea.style.display = 'none';
            
          // PERBAIKAN: Tampilkan kembali elemen "Hapus riwayat pencarian" dan "Sedang Trend"
if (clearHistoryBtn) clearHistoryBtn.style.display = 'block';
const trendPill = document.querySelector('.trend-pill');
if (trendPill) trendPill.style.display = 'inline-flex';
          
            // PERBAIKAN: Nonaktifkan mode khusus
            deactivateSpecialModes();
            
            // PERBAIKAN PENTING: Pastikan keyword suggestions benar-benar tersembunyi
            keywordSuggestions.style.display = 'none';
            keywordSuggestions.style.visibility = 'hidden';
            keywordSuggestions.style.opacity = '0';
            keywordSuggestions.style.height = '0';
            keywordSuggestions.style.overflow = 'hidden';
            keywordSuggestions.style.pointerEvents = 'none';
            
            // Reset additional suggestions ke tertutup
            additionalSuggestions.classList.remove('open');
            
            // Kembalikan tombol Lihat Lainnya ke posisi awal
            if (document.querySelector('.see-more-container').parentNode === additionalSuggestions) {
                textSuggestions.parentNode.insertBefore(document.querySelector('.see-more-container'), additionalSuggestions);
                seeMoreBtn.innerHTML = '<i class="fa fa-plus-circle"></i><span>Lihat Lainnya</span>';
            }
            
            // PERBAIKAN: Tampilkan placeholder jika input kosong
            expandedSearchPlaceholder.style.display = 'flex';
            
            // PERBAIKAN: Reset kolom pencarian expanded
            expandedSearchInput.value = '';
            expandedSearchPlaceholder.classList.remove('force-hide-placeholder');
            expandedClearSearchIcon.style.display = 'none';
            
            // PERBAIKAN: Sembunyikan tab filter saat tampilan awal
            forceHideFilterTabs();
            
            // PERBAIKAN: Reset isSearchResultShown
            isSearchResultShown = false;
            
            // PERBAIKAN: Tampilkan kembali semua elemen sugesti
            textSuggestions.style.display = 'block';
            document.querySelector('.see-more-container').style.display = 'block';
            document.querySelector('.product-title').style.display = 'block';
            
            // Pastikan tampilan responsive
            suggestionsContainer.style.width = '100%';
            suggestionsContainer.style.maxWidth = '800px';
            suggestionsContainer.style.margin = '0 auto'; // Disesuaikan karena posisi absolute
            suggestionsContainer.style.boxSizing = 'border-box';
            suggestionsContainer.style.position = 'absolute';
            suggestionsContainer.style.top = '60px'; // FIX: Pastikan menempel pada search bar
            suggestionsContainer.style.left = '0';
            suggestionsContainer.style.right = '0';
            
            expandedSearchInput.focus();
            
            // Tampilkan sugesti
            suggestionsContainer.style.display = 'block';
            
            // PERBAIKAN: Pastikan tampilan produk lainnya dan peringatan disembunyikan
    productsContainer.style.display = 'none';
    notFoundContainer.style.display = 'none';
    document.getElementById('productHeader').style.display = 'none';
    
    // PERBAIKAN: Tampilkan produk dengan layout simpel (hanya gambar dan judul 3 kata)
    renderSimpleProducts();
            
            // PERBAIKAN PENTING: Tambahan timer untuk memastikan keyword suggestions tersembunyi
            setTimeout(function() {
                keywordSuggestions.style.display = 'none';
                keywordSuggestions.style.visibility = 'hidden';
                keywordSuggestions.style.opacity = '0';
            }, 50);
            
            setTimeout(function() {
                keywordSuggestions.style.display = 'none';
                keywordSuggestions.style.visibility = 'hidden';
                keywordSuggestions.style.opacity = '0';
            }, 300);
            
            setTimeout(function() {
                keywordSuggestions.style.display = 'none';
                keywordSuggestions.style.visibility = 'hidden';
                keywordSuggestions.style.opacity = '0';
            }, 1000);
        }
// Sembunyikan mode pencarian
        function hideSearchMode() {
            searchOverlay.style.display = 'none';
            mainHeader.style.display = 'block';
            contentArea.style.display = 'block';
            
            // PERBAIKAN: Nonaktifkan mode khusus
            deactivateSpecialModes();
            
            // Tampilkan keyword suggestions berdasarkan ukuran layar
            if (window.innerWidth <768) {
                keywordSuggestions.style.display = 'block';
            } else {
                keywordSuggestions.style.display = 'none';
            }
            
            suggestionsContainer.style.display = 'none';
            
            // PERBAIKAN: Teks pencarian tidak dibawa ke tampilan awal
            searchInput.value = '';
        }

        // Fungsi untuk mengisi kolom pencarian dengan teks sugesti
        function fillSearchWithSuggestion(text) {
            expandedSearchInput.value = text;
            expandedSearchPlaceholder.classList.add('force-hide-placeholder');
            // PERBAIKAN: Tampilkan ikon silang
            expandedClearSearchIcon.style.display = 'flex';
            
            // PERBAIKAN: Langsung jalankan pencarian setelah mengisi teks
    executeSearch();
}

        // PERBAIKAN: Jalankan pencarian - Mengubah ke card produk standar saat ikon search diklik
        function executeSearch() {
            const searchText = expandedSearchInput.value.trim();
            if (searchText) {
                // Set flag bahwa hasil pencarian sedang ditampilkan
                isSearchResultShown = true;
                
                // PERBAIKAN: Reset filter ke "terkait" saat pencarian baru
                currentFilter = 'terkait';
                updateFilterTabsUI();
                
                // PERBAIKAN: Tampilkan produk dengan tampilan standar (gambar, nama lengkap, harga)
                renderStandardProducts(searchText, true);
            } else {
                // Jika pencarian kosong, tampilkan produk dengan layout simpel
                renderSimpleProducts();
                
                // Reset flag pencarian
                isSearchResultShown = false;
                
                // PERBAIKAN: Nonaktifkan mode khusus
                deactivateSpecialModes();
                
                // PERBAIKAN: Sembunyikan tab filter
                forceHideFilterTabs();
                
                // PERBAIKAN: Tambahkan penundaan tambahan untuk memastikan tab filter tersembunyi
                setTimeout(forceHideFilterTabs, 50);
                setTimeout(forceHideFilterTabs, 200);
                setTimeout(forceHideFilterTabs, 500);
                
                // PERBAIKAN: Tampilkan kembali sugesti
                textSuggestions.style.display = 'block';
                document.querySelector('.see-more-container').style.display = 'block';
                document.querySelector('.product-title').style.display = 'block';
                
                // PERBAIKAN BUG: Sembunyikan teks oranye bergerak (keyword suggestions)
                keywordSuggestions.style.display = 'none';
                
                // PERBAIKAN BUG: Pastikan placeholder tetap terlihat dengan benar
                expandedSearchPlaceholder.classList.remove('force-hide-placeholder');
            }
        }
        
        // PERBAIKAN 4: Update UI untuk tab filter
        function updateFilterTabsUI() {
            // Jangan lakukan apa-apa jika tidak ada pencarian aktif
            if (!isSearchResultShown) {
                forceHideFilterTabs();
                return;
            }
            
            // Jangan tampilkan filter jika dalam mode khusus
            if (searchOverlay.classList.contains('not-found-mode') || 
                searchOverlay.classList.contains('other-products-mode')) {
                forceHideFilterTabs();
                return;
            }
            
            // Pastikan filter tabs terlihat hanya saat pencarian aktif
            forceShowFilterTabs();
            
            // Hapus kelas active dari semua tab
            document.querySelectorAll('.filter-tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Tambahkan kelas active ke tab yang dipilih
            const activeTab = document.querySelector(`.filter-tab[data-filter="${currentFilter}"]`);
            if (activeTab) {
                activeTab.classList.add('active');
            }
            
            // Update ikon sort untuk tab harga jika dipilih
            if (currentFilter === 'harga') {
                priceSort.className = priceSortDirection === 'asc' ? 'fa fa-sort-up' : 'fa fa-sort-down';
            } else {
                priceSort.className = 'fa fa-sort';
            }
        }

        // PERBAIKAN: Implementasi yang diperbaiki untuk tombol "Lihat Lainnya"
        function toggleAdditionalSuggestions() {
            // Log untuk debugging
            console.log("Tombol 'Lihat Lainnya' diklik");
            
            // Sembunyikan placeholder
            expandedSearchPlaceholder.classList.add('force-hide-placeholder');
            
            // Toggle kelas open untuk additional suggestions
            additionalSuggestions.classList.toggle('open');
            const isOpen = additionalSuggestions.classList.contains('open');
            
            // Log untuk debugging
            console.log("Additional suggestions terbuka:", isOpen);
            
            if (isOpen) {
                // Pindahkan tombol ke dalam additional suggestions
                additionalSuggestions.appendChild(document.querySelector('.see-more-container'));
                // Ubah teks menjadi "Sembunyikan"
                seeMoreBtn.innerHTML = '<i class="fa fa-minus-circle"></i><span>Sembunyikan</span>';
            } else {
                // Pindahkan tombol kembali ke posisi awal
                const textSuggestions = document.querySelector('.text-suggestions');
                if (textSuggestions && textSuggestions.parentNode) {
                    textSuggestions.parentNode.insertBefore(document.querySelector('.see-more-container'), additionalSuggestions);
                }
                // Ubah teks kembali menjadi "Lihat Lainnya"
                seeMoreBtn.innerHTML = '<i class="fa fa-plus-circle"></i><span>Lihat Lainnya</span>';
            }
        }
        
// Event listeners
        searchInput.addEventListener('click', showSearchMode);
        
        // Event ketika tekan Enter di expanded search input
        expandedSearchInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                executeSearch();
            }
        });
        
        // Klik tombol kembali
        backBtn.addEventListener('click', hideSearchMode);
        
        // PERBAIKAN: Klik tombol search - Memaksa tab filter muncul
        expandedSearchIcon.addEventListener('click', function(e) {
            e.preventDefault();
            e.stopPropagation();
            console.log("Tombol search diklik");
            
            // Eksekusi pencarian
            executeSearch();
        });
        
        // PERBAIKAN: Tambahkan event listener baru untuk tombol "Lihat Lainnya"
        seeMoreBtn.addEventListener('click', function(e) {
            e.preventDefault();
            e.stopPropagation();
            toggleAdditionalSuggestions();
        });
        
        // PERBAIKAN 4: Event listener untuk tab filter
        document.querySelectorAll('.filter-tab').forEach(tab => {
            tab.addEventListener('click', function(e) {
                e.preventDefault();
                e.stopPropagation();
                
                console.log("Filter tab diklik:", this.getAttribute('data-filter'));
                
                const filterType = this.getAttribute('data-filter');
                
                if (filterType === 'harga' && currentFilter === 'harga') {
                    // Toggle arah pengurutan untuk harga
                    priceSortDirection = priceSortDirection === 'asc' ? 'desc' : 'asc';
                } else {
                    currentFilter = filterType;
                }
                
                // Update UI
                updateFilterTabsUI();
                
                // Re-render produk dengan filter baru
                const searchText = expandedSearchInput.value.trim();
                if (searchText) {
                    renderStandardProducts(searchText, true);
                }
            });
        });
        
        // Event listener untuk item sugesti
        suggestionItems.forEach(item => {
            item.addEventListener('click', function() {
                const suggestionText = this.getAttribute('data-text');
                fillSearchWithSuggestion(suggestionText);
            });
        });
        // Event listener khusus untuk overlay - TAMBAHKAN INI SEBELUM EVENT LISTENER DOCUMENT
searchOverlay.addEventListener('click', function(event) {
    // Periksa apakah klik langsung pada overlay itu sendiri, bukan pada elemen di dalamnya
    if (event.target === searchOverlay) {
        // Hentikan propagasi event agar tidak mencapai document event listener
        event.stopPropagation();
        // Mencegah default behavior
        event.preventDefault();
    }
});

// Event listener untuk klik di luar area pencarian - INI KODE YANG SUDAH ADA, DENGAN SEDIKIT MODIFIKASI
document.addEventListener('click', function(event) {
    // Tambahkan pengecekan untuk mengabaikan klik pada overlay itu sendiri
    if (event.target === searchOverlay) {
        return; // Keluar dari fungsi tanpa melakukan apa-apa
    }
    
    // Bagian di bawah ini tetap sama seperti sebelumnya
    const isClickInsideSearch = 
        expandedSearchInput.contains(event.target) || 
        backBtn.contains(event.target) ||
        expandedSearchIcon.contains(event.target) ||
        expandedClearSearchIcon.contains(event.target) || 
        filterTabs.contains(event.target) ||
        event.target.closest('.filter-tab') ||
        event.target === searchInput;
    
    const isClickInsideSuggestions = suggestionsContainer.contains(event.target);
    
    if (searchOverlay.style.display === 'block' && !isClickInsideSearch && !isClickInsideSuggestions) {
        // Pastikan tidak klik pada elemen-elemen yang terkait dengan pencarian
        if (!event.target.closest('.product-card') && 
            !event.target.closest('.simple-product-card') &&
            !event.target.closest('.suggestion-item') &&
            !event.target.closest('.see-more-btn') &&
            !event.target.closest('.filter-tab')) {
            hideSearchMode();
        }
    }
});
       
// Event listener untuk additional suggestions
        const additionalSuggestionItems = additionalSuggestions.querySelectorAll('.suggestion-item');
        additionalSuggestionItems.forEach(item => {
            item.addEventListener('click', function() {
                const suggestionText = this.getAttribute('data-text');
                fillSearchWithSuggestion(suggestionText);
            });
        });
        
        // Event listener untuk tombol "Produk Lainnya"
        document.addEventListener('click', function(event) {
            const target = event.target;
            const isProductAction = target.classList.contains('product-action') || 
                                    target.closest('.product-action');
            
            if (isProductAction) {
                showOtherProducts();
            }
        });

        // Event listener untuk tombol "Coba kata kunci lain"
        if (tryAnotherKeywordBtn) {
            tryAnotherKeywordBtn.addEventListener('click', function() {
                // PERBAIKAN: Nonaktifkan mode khusus sebelum membersihkan
                deactivateSpecialModes();
                
                clearSearchInput();
            });
        }

        // Event listener untuk tombol "Coba produk lainnya"
        if (trySuggestionBtn) {
            trySuggestionBtn.addEventListener('click', function() {
                document.body.classList.add('show-suggestions');
                keywordSuggestionsPopup.style.display = 'block';
                suggestionsOverlay.style.display = 'block';
            });
        }
        
        // Event listener untuk overlay sugesti
        if (suggestionsOverlay) {
            suggestionsOverlay.addEventListener('click', function() {
                document.body.classList.remove('show-suggestions');
                keywordSuggestionsPopup.style.display = 'none';
                suggestionsOverlay.style.display = 'none';
            });
        }
        
        // Event listener untuk keyword tag dalam popup
        document.querySelectorAll('.keyword-suggestion-tag').forEach(tag => {
            tag.addEventListener('click', function() {
                const productName = this.getAttribute('data-product') || this.textContent.trim();
                
                // Sembunyikan popup
                document.body.classList.remove('show-suggestions');
                keywordSuggestionsPopup.style.display = 'none';
                suggestionsOverlay.style.display = 'none';
                
                // Isi input pencarian dan tampilkan ikon silang
                expandedSearchInput.value = productName;
                expandedSearchPlaceholder.classList.add('force-hide-placeholder');
                expandedClearSearchIcon.style.display = 'flex';
                
                // PERBAIKAN: Tampilkan produk dengan layout simpel
                executeSearch();
            });
        });
        
        // Event listener untuk product details
        document.addEventListener('DOMContentLoaded', function() {
            const productDetails = document.getElementById('productDetails');
            if (productDetails) {
                productDetails.addEventListener('click', function() {
                    window.location.href = 'https://www.facebook.com';
                });
            }
        });
        
        // PERBAIKAN: Inisialisasi tampilan awal
        renderSimpleProducts();
        
        // PERBAIKAN BUG: Pastikan tab filter tersembunyi saat halaman dimuat
        document.addEventListener('DOMContentLoaded', function() {
            // Pastikan tab filter tersembunyi saat awal
            forceHideFilterTabs();
            
            // PERBAIKAN: Pastikan tidak ada mode khusus aktif
            deactivateSpecialModes();
            
            // Tambahkan pengecekan penundaan untuk memastikan
            setTimeout(forceHideFilterTabs, 100);
            setTimeout(forceHideFilterTabs, 500);
            setTimeout(forceHideFilterTabs, 1000);
            
            // FIX: Pastikan suggestions container akan selalu menempel pada search bar expanded
            // dengan menerapkan posisi absolute dan menghilangkan jarak
            if (suggestionsContainer) {
                suggestionsContainer.style.position = 'absolute';
                suggestionsContainer.style.top = '60px';
                suggestionsContainer.style.left = '0';
                suggestionsContainer.style.right = '0';
                suggestionsContainer.style.marginTop = '0';
                
                // FIX: Tambahkan event listener untuk tetap menjaga posisi
                // suggestions container saat layar diubah ukurannya
                window.addEventListener('resize', function() {
                    suggestionsContainer.style.position = 'absolute';
                    suggestionsContainer.style.top = '60px';
                    suggestionsContainer.style.left = '0';
                    suggestionsContainer.style.right = '0';
                    suggestionsContainer.style.marginTop = '0';
                });
            }
        });
        
        // PERBAIKAN PENTING: Tambahkan mutationObserver untuk memantau perubahan tampilan
        const searchObserver = new MutationObserver(function(mutations) {
            mutations.forEach(function(mutation) {
                if (mutation.attributeName === 'style' && 
                    searchOverlay.style.display === 'block') {
                    // Ketika tampilan 2 aktif, paksa sembunyikan keyword suggestions
                    if (keywordSuggestions) {
                        keywordSuggestions.style.display = 'none';
                        keywordSuggestions.style.visibility = 'hidden';
                        keywordSuggestions.style.opacity = '0';
                        keywordSuggestions.style.height = '0';
                        keywordSuggestions.style.overflow = 'hidden';
                        keywordSuggestions.style.pointerEvents = 'none';
                    }
                    
                    // FIX: Pastikan suggestions container selalu menempel pada expanded search bar
                    if (suggestionsContainer) {
                        suggestionsContainer.style.position = 'absolute';
                        suggestionsContainer.style.top = '60px';
                        suggestionsContainer.style.left = '0';
                        suggestionsContainer.style.right = '0';
                        suggestionsContainer.style.marginTop = '0';
                    }
                    
                    // Periksa jika mode khusus aktif
                    if (searchOverlay.classList.contains('not-found-mode') || 
                        searchOverlay.classList.contains('other-products-mode')) {
                        forceHideFilterTabs();
                    }
                }
            });
        });

        // Mulai mengamati perubahan pada searchOverlay
        if (searchOverlay) {
            searchObserver.observe(searchOverlay, { attributes: true });
        }

        // Tambahkan event click khusus untuk ikon pencarian yang diperluas
        if (expandedSearchIcon) {
            expandedSearchIcon.addEventListener('click', function() {
                // Paksa sembunyikan keyword suggestions saat ikon diklik
                if (keywordSuggestions) {
                    keywordSuggestions.style.display = 'none';
                    keywordSuggestions.style.visibility = 'hidden';
                    keywordSuggestions.style.opacity = '0';
                    keywordSuggestions.style.height = '0';
                    keywordSuggestions.style.overflow = 'hidden';
                    keywordSuggestions.style.pointerEvents = 'none';
                }
                
                // FIX: Pastikan suggestions container selalu menempel pada search bar
                if (suggestionsContainer) {
                    suggestionsContainer.style.position = 'absolute';
                    suggestionsContainer.style.top = '60px';
                    suggestionsContainer.style.left = '0';
                    suggestionsContainer.style.right = '0';
                    suggestionsContainer.style.marginTop = '0';
                }
            }, true);
        }

        // FIX: Tambahkan event listener saat input expanded mendapat focus
        expandedSearchInput.addEventListener('focus', function() {
            // Pastikan suggestions container menempel pada search bar
            if (suggestionsContainer) {
                suggestionsContainer.style.position = 'absolute';
                suggestionsContainer.style.top = '60px';
                suggestionsContainer.style.left = '0';
                suggestionsContainer.style.right = '0';
                suggestionsContainer.style.marginTop = '0';
            }
        });

        // FIX: Tambahkan event listener untuk perubahan input
        expandedSearchInput.addEventListener('input', function() {
            // Pastikan suggestions container menempel pada search bar
            if (suggestionsContainer) {
                suggestionsContainer.style.position = 'absolute';
                suggestionsContainer.style.top = '60px';
                suggestionsContainer.style.left = '0';
                suggestionsContainer.style.right = '0';
                suggestionsContainer.style.marginTop = '0';
            }
            
            // Toggle ikon silang berdasarkan isi input
            toggleClearIcon(this);
        });
        
        // PERBAIKAN: Tambahkan MutationObserver untuk memantau perubahan class pada searchOverlay
        const classObserver = new MutationObserver(function(mutations) {
            mutations.forEach(function(mutation) {
                if (mutation.attributeName === 'class') {
                    // Jika mode khusus aktif, pastikan tab filter tersembunyi
                    if (searchOverlay.classList.contains('not-found-mode') || 
                        searchOverlay.classList.contains('other-products-mode')) {
                        forceHideFilterTabs();
                    }
                }
            });
        });
        
        // Mulai memantau perubahan class pada searchOverlay
        if (searchOverlay) {
            classObserver.observe(searchOverlay, { attributes: true, attributeFilter: ['class'] });
        }
        
        // PERBAIKAN: Tambahkan MutationObserver untuk notFoundContainer
        const notFoundObserver = new MutationObserver(function(mutations) {
            mutations.forEach(function(mutation) {
                if (mutation.attributeName === 'style') {
                    // Jika notFoundContainer ditampilkan, aktifkan mode khusus
                    if (notFoundContainer.style.display === 'block') {
                        activateNotFoundMode();
                    }
                }
            });
        });
        
        // Mulai memantau perubahan style pada notFoundContainer
        if (notFoundContainer) {
            notFoundObserver.observe(notFoundContainer, { attributes: true, attributeFilter: ['style'] });
        }
        
        // PERBAIKAN: Tambahkan MutationObserver untuk productsContainer
        const productsObserver = new MutationObserver(function(mutations) {
            mutations.forEach(function(mutation) {
                if (mutation.attributeName === 'style') {
                    // Jika productsContainer ditampilkan, aktifkan mode khusus
                    if (productsContainer.style.display === 'block') {
                        activateOtherProductsMode();
                    }
                }
            });
        });
        
        // Mulai memantau perubahan style pada productsContainer
        if (productsContainer) {
            productsObserver.observe(productsContainer, { attributes: true, attributeFilter: ['style'] });
        }
  
  // Fungsi untuk menghitung jumlah keyword trend dan update badge
function updateTrendCount() {
    // Hitung jumlah item dalam additional suggestions
    const trendItems = document.querySelectorAll('.additional-suggestions .suggestion-item');
    const trendCount = trendItems.length;
    
    // Update badge dengan jumlah
    const trendBadge = document.getElementById('trendCount');
    if (trendBadge) {
        trendBadge.textContent = trendCount;
    }
}

// Jalankan fungsi saat halaman dimuat
document.addEventListener('DOMContentLoaded', function() {
    updateTrendCount();
});
    </script>
</body>
</html>