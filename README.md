# Multithreaded-file-compression-tool

COMPANY NAME -CODTECH IT SOLUTIONS

NAME -Arjun Ganpati Waghmode

INTERN ID -CT08DK949

DOMAIN NAME -C++ PROGRAMMING

DURATION -8 WEEKS(APRIL 25th to JUNE 25th 2025)

MENTOR -NEELA SANTHOSH KUMAR

📌 Overview


This C++ program is a multithreaded file compressor and decompressor that uses the Zlib library to apply compression algorithms and manage files in chunks. It allows users to choose between compressing or decompressing a file using terminal input. Here's what it does:

📂 Reads a file in chunks (1 MB by default)

🧵 Spawns multiple threads to compress or decompress each chunk in parallel

💾 Writes the results in order to an output file

⚡ Designed for performance and order-preserving output


🔧 How It Works – Compression


Step-by-step Process:
User Input:

Asks the user to choose whether to compress or decompress a file via terminal input.

File Reading:

If the user selects compression, the program reads the file input.txt in chunks of 1 MB.

Each chunk is stored in a Chunk structure with its binary data and index (used for maintaining order).

Compression Threads:

For each chunk read, a new thread is spawned using compressChunk.

This function uses zlib's compress2 with Z_BEST_COMPRESSION to compress the chunk's data.

Queue Management:

Compressed chunks are placed in a thread-safe queue (compressedQueue) guarded by a mutex and condition_variable.

Threads notify a writer thread that data is available.

Ordered Writing:

A writerThread consumes from the queue and writes to the output file (output.z).

If a chunk arrives out of order, it's temporarily stored in a map until the missing chunk arrives (sorted via index).

Writing Format:

For each chunk: writes chunk index, chunk size, then actual data.

Ensures the decompressor can reconstruct the original file exactly.

Thread Management:

Waits for all threads to complete using join().

Notifies the writer thread to finish after all compression threads are done.

🔄 How It Works – Decompression


Step-by-step Process:
File Reading:

Opens the compressed file output.z.

Reads each chunk by first retrieving the index and size from the file.

Then reads the actual compressed data of the specified size.

Decompression Threads:

Spawns a thread per chunk, calling decompressChunk.

Uses zlib's uncompress() to decompress the chunk.

Stores decompressed chunks in a thread-safe queue.

Ordered Writing Again:

A writerThread writes to decompressed.txt in correct order.

Utilizes the same index ordering strategy as compression.

Thread Synchronization:

Waits for all decompression threads to finish.

Notifies the writer thread once done.

🧠 Learning Objectives


🎯 Key Concepts Taught by This Program:

1. Multithreading in C++
Using std::thread for parallel computation.

Handling synchronization with std::mutex and std::condition_variable.

2. Chunk-based File Processing
Reading large files in manageable chunks.

Parallel processing of independent parts of data.

3. Compression and Decompression with zlib
Efficient use of compress2() and uncompress().

Understanding buffer size management for both compression and decompression.

4. Thread-safe Communication
Sharing data between threads safely using queues.

Avoiding race conditions and deadlocks with proper locking.

5. Order-Preserving Output
Using std::map to reorder out-of-order results.

Ensuring original file reconstruction from unordered parallel tasks.

6. System Programming
Binary file I/O with ifstream and ofstream.

Low-level file operations including raw memory reads and writes.

📁 Key Structures Used

struct Chunk {
    std::vector<unsigned char> data;
    size_t index;
};
Holds a chunk of file data and its position in the overall file.

Ensures the order can be restored after multithreaded processing.

🧵 Thread Roles


Thread Type	Purpose
Compressor Thread	Compresses a chunk and pushes it into compressedQueue
Decompressor Thread	Decompresses a chunk and pushes it into decompressedQueue
Writer Thread	Pulls from queue, checks index order, and writes to file

🪢 Thread Synchronization Tools

std::mutex: Ensures exclusive access to shared queues.

std::condition_variable: Notifies the writer thread when data is available.

std::lock_guard and std::unique_lock: RAII wrappers for safe locking.

⚠ Error Handling

Checks return values of compress2() and uncompress():

Logs to std::cerr if any chunk fails to compress/decompress.

Ensures input and output files are successfully opened.

🧪 Testing Suggestion

Try with different sizes of input.txt, e.g.:

Small file (~1 KB)

Medium file (~10 MB)

Large file (>100 MB)

To observe:

Parallel speedup

Order preservation

Compression ratio

🧰 Dependencies


🔧 zlib must be installed and linked with the project.

Compile with:



g++ -std=c++17 -lz -pthread yourfile.cpp -o compressor


🧼 Improvements & Extensions

Thread Pooling:

Use a fixed-size thread pool instead of one thread per chunk.

Progress Reporting:

Show compression/decompression progress using a simple counter or progress bar.

Dynamic Chunk Sizing:

Adjust chunk size based on system memory and performance.

Exception Safety:

Wrap critical sections in try-catch blocks.

Gracefully handle unexpected failures.

Command-line File Selection:

Allow user to specify file names rather than hardcoding "input.txt".

🔍 Code Summary


Component	Description
main()	Entry point; asks user for operation.
compression()	Handles reading, spawning compression threads, and writing.
compressChunk()	Compresses a chunk using zlib.
writerThread()	Maintains correct write order and writes to file.
Decompression()	Reads chunk headers, decompresses, and writes in order.
decompressChunk()	Decompresses data using zlib.

🎉 Conclusion


This project demonstrates practical systems-level programming with performance optimization in mind. It combines:

✅ Multithreading
✅ Compression Algorithms
✅ Synchronization Techniques
✅ Binary File I/O
✅ Efficient Data Structures




output:




