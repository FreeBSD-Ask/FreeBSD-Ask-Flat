# 6.6 Compression and Decompression

Compression and decompression tools are fundamental tools in computer file management. Data compression exploits the statistical redundancy of information to reduce the storage space required for data representation through encoding algorithms. Depending on whether information loss is permitted, compression algorithms can be divided into two major categories: lossless compression and lossy compression. The tools covered in this section all employ lossless compression algorithms, meaning the decompressed data is identical to the original data.

Common lossless compression algorithms and their characteristics are as follows:

| Algorithm | Format Used | Technical Basis | Characteristics |
| --------- | ----------- | --------------- | --------------- |
| DEFLATE | zip/gzip | LZ77 + Huffman coding | Classic general-purpose |
| LZMA/LZMA2 | xz | LZMA chained compression | High compression ratio |
| LZ4 | lz4 | Byte-level LZ77 | Extremely high speed |
| Zstandard | zstd | Finite State Entropy + LZ77 | Balances speed and compression ratio |
| bzip2 | bz2 | Burrows-Wheeler Transform + Huffman | Higher compression ratio, slower speed |

## zip

The zip format is an implementation of the PKZIP archive format, whose specification is maintained by PKWARE (APPNOTE.TXT). The Info-ZIP project provides the open-source zip/unzip tool implementation. zip is a compression and file packaging tool compatible with PKZIP (Phil Katz's ZIP for MSDOS systems). zip version 3.0 is compatible with PKZIP 2.04 and supports the Zip64 extension (allowing archives and files to exceed the 2 GB limit). zip uses deflation as the default compression method, but can also store files without compression, automatically choosing the better option for each file.

The zip format is the most commonly used format on Windows, but has limited support for Unicode filenames (depending on the zip tool version and compression settings). When exchanging files across platforms, it is recommended to use the tar.xz or tar.zst format.

> **Tip**
>
> It is normal to encounter garbled characters when using zip to compress Chinese or non-English characters. Due to different encoding methods, when zip 3.0 is compiled on platforms that support Unicode, it additionally stores a UTF-8 translation of the path to improve cross-platform filename compatibility. When decompressing, you can use the `unzip -O` option to specify the filename encoding (e.g., `unzip -O GBK`), or use convmv to batch-convert decompressed garbled filenames.

### Installing zip

- Using pkg

```sh
# pkg install zip
```

- Using Ports

```sh
# cd /usr/ports/archivers/zip/
# make install clean
```

### zip Compression

```sh
$ zip test.zip test # Compress into a zip file
```

### zip Decompression

When decompressing zip files, you need to install the `unzip` tool (FreeBSD's base system includes `bsdunzip` based on libarchive, which has limited functionality; for full features, install the Info-ZIP version via `pkg install unzip`).

```sh
$ unzip test.zip # Decompress the zip file to the current path
$ unzip test.zip -d /home/ykla/test # Decompress to the specified path, -d stands for directory
```

The `-d` option of unzip is followed by a directory name, which can have a space (e.g., `-d /path`) or be directly adjacent (e.g., `-d/path`).

## tar

The base system includes `tar`, so no installation is required.

tar stands for "tape archive," originally used for file storage on magnetic tape. FreeBSD's tar implementation is based on the libarchive library (i.e., bsdtar), which replaced the GNU tar in earlier versions. This implementation can extract files from tar, pax, cpio, zip, jar, ar, xar, rar (including RAR2, RAR3, and most RAR5 formats, read-only due to proprietary format restrictions), rpm, 7-zip, and ISO 9660 disc images, and can create archives in tar, pax, cpio, ar, zip, 7-zip, and shar formats.

GNU tar supports automatic detection of multiple compression formats; bsdtar also automatically detects the compression format during decompression (without needing to manually specify `-z`, `-j`, `-J`, etc. options), and supports extraction from tar, pax, cpio, zip, and other formats, whereas GNU tar only supports tar-related formats.

> **Thought Question**
>
>> An archive file package is a collection of files with a compression ratio of `0`, meaning multiple files or directories are packaged into a single file for storage. Using `tar` alone only packages without compression. The essence of compression is to reduce the storage space occupied by files through algorithms, not to compress directories themselves. Therefore, common compression software typically first archives directories into files and then compresses them.
>>
>> How do you understand the relationship between archiving and compression?

### tar Compression

```sh
$ tar -cvf test.tar test # Package into a tar format file. -c stands for Create
$ tar -zcvf test.tar.gz test # Compress into a gzip format file. -z stands for gzip
$ tar -jcvf test.tar.bz2 test # Compress into a bzip2 format file. The -j parameter stands for bzip2, note the case
$ tar -Jcvf test.tar.xz test # Compress into an xz format file. The -J parameter stands for xz, note the case
$ tar --zstd -cvf test.tar.zst test # Compress into a zstd format file
```

### tar Decompression

```sh
$ tar -xvf test.tar.other-compression-format # Decompress a tar format file, supports formats such as test.tar.bz2, test.tar.gz, test.tar.xz, test.tar.zst, etc.
$ tar -xvf test.tar -C /home/ykla/mytest # Decompress test.tar to the specified path
```

Option descriptions:

| Option | Meaning |
| ------ | ------- |
| `x` | Extract, decompress |
| `v` | verbose, detailed output mode |
| `f` | file, specify the file |
| `C` | cd, specify the path |

## xz

The base system includes `xz` and `unxz`, so no installation is required either. The xz format is one of the formats with the highest compression ratio currently available, particularly suitable for archiving large files.

### `xz` Compression

By default, the original file is deleted after compression. It is recommended to add the `-k` option to keep the original file.

```sh
$ xz -k test.txt
```

Compress and delete the original file:

```sh
$ xz test.pdf
```

### `unxz` Decompression

unxz is actually a hard link to xz; using `xz -d` or directly `unxz` produces the same effect.

```sh
$ unxz -k test.tar.xz  # Decompress and keep the original file, the -k parameter stands for keep, same below
$ unxz test.tar.xz     # Decompress and delete the original file
```

## 7z

The 7z format offers an extremely high compression ratio, particularly suitable for archiving large files.

In the FreeBSD operating system, the 7z command can be used by installing the `archivers/7-zip` package.

### Installing 7-zip

- Using pkg:

```sh
# pkg install 7-zip
```

- Via Ports:

```sh
# cd /usr/ports/archivers/7-zip/
# make install clean
```

### 7z Compression

```sh
$ 7z a test.7z test # Compress the test file into a 7z file.
```

`a` stands for add, adding the file to be compressed to test.7z.

### 7z Decompression

```sh
$ 7z x test.7z # Decompress the 7z file
$ 7z x test.7z -o/home/ykla/Downloads/test # Decompress test.7z to the specified path
```

`-o` stands for Output, specifying the output path.

> **Warning**
>
> In `-o/home/ykla/Downloads/test`, there must be **no space** between `-o` and the path. This is not a typo but the design of the 7z command. PRs for improvement are welcome.

## rar

rar is one of the most popular formats on Windows, but has lower usage in the Unix world; rar has good recovery records and volume splitting features, making it suitable for large file transfers.

The rar format is proprietary and is not included in the base system.

### Installing rar

- Install via pkg:

```sh
# pkg install rar unrar
```

- Via Ports:

```sh
# cd /usr/ports/archivers/rar/ && make install clean
# cd /usr/ports/archivers/unrar/ && make install clean
```

### rar Compression

```sh
$ rar a archive.rar test
```

`a` stands for add, adding the file to `archive.rar`.

### rar Decompression

```sh
$ unrar x archive.rar # Decompress to the current path. The x command stands for Extract, decompress
$ unrar x archive.rar /home/ykla/Desktop/test/ # Decompress to the specified directory
```

## zstd

zstd is a fast compression algorithm developed by Meta (formerly Facebook) that balances compression speed and compression ratio, making it the preferred compression format for modern systems.

zstd is also a compression tool included in the base system. zstd supports multiple preset levels, from the fastest (`-1`) to extreme compression (`--ultra -22`).

### zstd Compression

- Compress a single file using zstd

```sh
$ zstd test.pdf
```

- Compress a folder using zstd.

zstd does not directly support compressing folders (see: GitHub. How can I compress a directory?[EB/OL]. [2026-03-26]. <https://github.com/facebook/zstd/issues/1526>.). This Issue discusses the technical reasons why zstd does not support direct directory compression and alternative approaches, so you need to first package the folder into a tar file.

> **Thought Question**
>
> Why doesn't zstd support compressing folders? What are the possible reasons?

```sh
$ tar -cf test.tar /home/ykla/test/ # First package into tar. The -f parameter stands for file
```

Compress `test.tar` into `test.tar.zst`

```sh
$ zstd -o test.tar.zst test.tar # The -o parameter stands for file, used to specify the output file
```

Additionally, you can directly use tar's `--zstd` option to complete packaging and compression in one step:

```sh
$ tar --zstd -cvf output-file -C starting-directory relative-path
```

Example:

```sh
$ tar --zstd -cvf test.tar.zst -C /home/ykla/ test # Package and compress into zstd format
a test
```

View the result:

```sh
$ ls -al
drwxr-xr-x  2 ykla ykla     2 Apr 16 11:53 test
-rw-r--r--  1 ykla ykla    98 Apr 16 11:58 test.tar.zst
```

File structure diagram:

```sh
Current working directory (where the command is executed, e.g., ~)
│
├── test.tar.zst   ← Output file (always generated in the "current directory")
│
└── (The -C option will change the directory before adding subsequent files)
     ↓
     Switch to: /home/ykla/
                │
                └── test   ← The object being packaged
```

### zstd Decompression

#### Decompress to the Current Path

```sh
$ zstd -d test.tar.zst
```

> **Tip**
>
> The result of this decompression is `test.tar`, which still needs to be decompressed once more using `tar`.

#### Decompress to a Specified Path

```sh
$ zstd -d test.tar.zst -o /home/ykla/mytest # The -d parameter stands for decompress
```

> **Tip**
>
> Same as above, the decompressed result is `test.tar`, which still needs to be decompressed once more using `tar`.

## Exercises

1. Use tools such as zip, tar, xz, 7z, rar, and zstd to compress and decompress files containing Chinese filenames respectively, compare how each tool handles UTF-8 encoding, and analyze the design differences in internationalization support across different compression formats.
2. Use different compression algorithms (gzip, bzip2, xz, zstd) to compress the same set of files, compare the compression ratio, compression time, and decompression time, and analyze the trade-off strategies between compression efficiency and computational performance for each algorithm.
3. Review the clauses regarding filename encoding in the zip format specification, analyze the root causes of Chinese filename garbling issues, and evaluate the applicability of existing patch solutions.
