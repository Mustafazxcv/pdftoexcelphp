# PDF to Excel Converter (PDF'den Excel'e Dönüştürücü)

Bu PHP betiği, Smalot/PdfParser ve PhpSpreadsheet kütüphanelerini kullanarak PDF dosyalarını Excel dosyalarına dönüştürür.

## Gereksinimler
- Composer (Paket yöneticisi)
- Smalot/PdfParser
- PhpSpreadsheet

## Kullanım
1. `example3.pdf` adlı PDF dosyasını dönüştürmek için betiği çalıştırın.
2. Dönüştürülen Excel dosyası otomatik olarak `output.xlsx` olarak kaydedilecek.
3. Kullanıcıya indirme bağlantısı sunulacaktır.

## Örnek Kullanım
```php
<?php
require 'vendor/autoload.php';

use Smalot\PdfParser\Parser;
use PhpOffice\PhpSpreadsheet\Spreadsheet;
use PhpOffice\PhpSpreadsheet\Writer\Xlsx;

$pdf_file = 'example3.pdf';

// PDF dosyasından metin içeriğini alma
$parser = new Parser();
$pdf = $parser->parseFile($pdf_file);
$text = $pdf->getText();

// Excel dosyası oluşturma
$spreadsheet = new Spreadsheet();
$sheet = $spreadsheet->getActiveSheet();

// Metni satır ve sütunlara ayırarak Excel dosyasına yazma
$rowIndex = 1;
foreach (explode("\n", $text) as $line) {
    $colIndex = 1;
    foreach (explode("\t", $line) as $cell) {
        $sheet->setCellValueByColumnAndRow($colIndex, $rowIndex, $cell);
        $colIndex++;
    }
    $rowIndex++;
}

// Excel dosyasını kaydetme
$excel_file = 'output.xlsx';
$writer = new Xlsx($spreadsheet);
$writer->save($excel_file);

// İndirme bağlantısı oluşturma ve kullanıcıya sunma
echo 'PDF dosyası başarıyla Excel dosyasına aktarıldı. <a href="' . $excel_file . '">İndirmek için tıklayınız</a>';
?>
