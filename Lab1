import os
import re
import sys

# read file with os system calls and return content
def read_file(filename):
    try:
	# open file (read only)
        fd = os.open(filename, os.O_RDONLY)
        content = b""
	# read file by chunks
        while True:
            chunk = os.read(fd, 4096)
            if not chunk:
                break 		# exit loop when no more data
            content += chunk	# append chunks
        os.close(fd)		# close descriptor for file
        return content.decode('utf-8', errors='ignore') # send content as decoded string
    except OSError as e:	# handle errors
        os.write(2, f"Error reading file {filename}: {e}\n".encode())
        sys.exit(1)

# write to output file with os system calls, overwrites file if it exists.
def write_file(filename, content):
    try:
	# open file, write only, create it if needed, overwrite, file permissions  rw-r-r
        fd = os.open(filename, os.O_WRONLY | os.O_CREAT | os.O_TRUNC, 0o644)
        total_bytes_written = 0
        buffer = content.encode('utf-8') # encode content back to bytes to write into file
        while buffer:
            bytes_written = os.write(fd, buffer)
            buffer = buffer[bytes_written:]
            total_bytes_written += bytes_written # keep track of bytes
        os.fsync(fd) # write data to disk
        os.close(fd) # close file descriptor
        os.write(1, f"Wrote a total of {total_bytes_written} bytes\n".encode())
    except OSError as e:
        os.write(2, f"Error writing to file {filename}: {e}\n".encode())
        sys.exit(1)

def count_words(text):
	# find all words (only letters), handle case sensitivity by transforming to lowercase
    words = re.findall(r'\b[a-zA-Z]+\b', text.lower())
    word_count = {}

	# count number of times a word appears in the file and return count
    for word in words:
        word_count[word] = word_count.get(word, 0) + 1
    return word_count

def main():
    if len(sys.argv) != 3: # check the needed args when running program
        os.write(2, b"Usage: python wordCount.py input.txt output.txt\n")
        sys.exit(1)
    
    input_file = sys.argv[1]
    output_file = sys.argv[2]
    
    text = read_file(input_file)
    word_count = count_words(text)
    
	# sort by word in descending alphabetical order  (1 to sort by word count)
    sorted_words = sorted(word_count.items(), key=lambda x: x[0], reverse=False)
    output_content = "\n".join(f"{word} {count}" for word, count in sorted_words)
    
    write_file(output_file, output_content)

if __name__ == "__main__":
    main()

