```Python
import requests
import sys
import time

def downloadFile(url, directory) :
  localFilename = url.split('/')[-1]
  print(url)
  with open(directory + '/' + localFilename, 'wb') as f:
    start = time.clock()
    r = requests.get(url, stream=True)
    total_length = r.headers.get('content-length')
    dl = 0
    if total_length is None: # no content length header
      f.write(r.content)
    else:
      for chunk in r.iter_content(1024):
        dl += len(chunk)
        f.write(chunk)
        done = 50 * dl / int(total_length)
        done = int(done)
        sys.stdout.write("\r[%s%s] %s bps" % ('=' * done, ' ' * (50-done), dl//(time.clock() - start)))
        print("")
  return (time.clock() - start)

def main() :
  if len(sys.argv) > 1 :
        url = sys.argv[1]
  else :
        url = input("Enter the URL : ")
  directory = input("Where would you want to save the file ?")

  time_elapsed = str(downloadFile(url, directory))
  print( "Download complete...")
  print ("Time Elapsed: " + time_elapsed)


if __name__ == "__main__" :
  main()
```