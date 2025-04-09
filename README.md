# sistemmanajemenkaryawan
using System;

namespace AplikasiGaji
{
    public abstract class Karyawan
    {
        protected string Nama { get; }
        protected string ID { get; }
        protected double GajiPokok { get; }

        protected Karyawan(string nama, string id, double gajiPokok)
        {
            Nama = nama;
            ID = id;
            GajiPokok = gajiPokok;
        }

        public string GetNama() => Nama;
        public string GetID() => ID;
        public double GetGajiPokok() => GajiPokok;

        public abstract double HitungTotalGaji();
    }

    public class KaryawanTetap : Karyawan
    {
        private const double TambahanTetap = 500000;

        public KaryawanTetap(string nama, string id, double gajiPokok)
            : base(nama, id, gajiPokok) { }

        public override double HitungTotalGaji()
        {
            return GajiPokok + TambahanTetap;
        }
    }

    public class KaryawanKontrak : Karyawan
    {
        private const double Potongan = 200000;

        public KaryawanKontrak(string nama, string id, double gajiPokok)
            : base(nama, id, gajiPokok) { }

        public override double HitungTotalGaji()
        {
            return GajiPokok - Potongan;
        }
    }

    public class Magang : Karyawan
    {
        public Magang(string nama, string id, double gajiPokok)
            : base(nama, id, gajiPokok) { }

        public override double HitungTotalGaji()
        {
            return GajiPokok;
        }
    }

    public class SistemGaji
    {
        public static void Mulai()
        {
            while (true)
            {
                TampilkanMenu();
                string pilihan = Console.ReadLine();

                if (pilihan == "4")
                {
                    Console.WriteLine("Terima kasih telah menggunakan aplikasi.");
                    break;
                }

                InputDataKaryawan(pilihan);
            }
        }

        private static void TampilkanMenu()
        {
            Console.Clear();
            Console.WriteLine("=== Menu Gaji Karyawan ===");
            Console.WriteLine("1. Tambah Karyawan Tetap");
            Console.WriteLine("2. Tambah Karyawan Kontrak");
            Console.WriteLine("3. Tambah Karyawan Magang");
            Console.WriteLine("4. Keluar");
            Console.Write("Pilih opsi (1-4): ");
        }

        private static void InputDataKaryawan(string pilihan)
        {
            Console.Write("\nNama Karyawan: ");
            string nama = Console.ReadLine();
            Console.Write("Nomor Identitas: ");
            string id = Console.ReadLine();

            double gaji;
            while (true)
            {
                Console.Write("Gaji Pokok: Rp.");
                if (double.TryParse(Console.ReadLine(), out gaji)) break;
                Console.WriteLine("Masukkan angka yang valid.");
            }

            Karyawan karyawan = BuatKaryawan(pilihan, nama, id, gaji);

            if (karyawan != null)
            {
                TampilkanGaji(karyawan);
            }
            else
            {
                Console.WriteLine("Opsi tidak valid. Tekan Enter untuk lanjut...");
                Console.ReadLine();
            }
        }

        private static Karyawan BuatKaryawan(string kode, string nama, string id, double gaji)
        {
            switch (kode)
            {
                case "1": return new KaryawanTetap(nama, id, gaji);
                case "2": return new KaryawanKontrak(nama, id, gaji);
                case "3": return new Magang(nama, id, gaji);
                default: return null;
            }
        }

        private static void TampilkanGaji(Karyawan k)
        {
            Console.WriteLine($"\nRincian Gaji untuk {k.GetNama()}:");
            Console.WriteLine($"ID: {k.GetID()}");
            Console.WriteLine($"Gaji Pokok: Rp.{k.GetGajiPokok()}");
            Console.WriteLine($"Total Gaji Diterima: Rp.{k.HitungTotalGaji()}");
            Console.WriteLine("\nTekan Enter untuk kembali ke menu.");
            Console.ReadLine();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            SistemGaji.Mulai();
        }
    }
}

