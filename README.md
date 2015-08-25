import urllib2
import string

def fetch(url):
    return urllib2.urlopen(url).read()
#-------------------------------------------------
def parseHtml(response):
    response = response.split("\n")
    html = "<html lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n</head>\n<body>\n"
    for line in response:
        if "View full resolution" in line:
            line = line.replace("href", "src")
            line = line.replace( "//", "http://")
            line = line.replace("<a", "<img")
            line = line.replace("target", ">")
            line = line[:line.find(">") + 1]
            line = line.strip()
            html = html + "<div>" + line + "</div><br/>\n"

    html = html + "</body>\n</html>"
    return html
#-------------------------------------------------
def writeFile(content):
    file = open("temp.html","w")
    file.write(content)
    file.close()
#-------------------------------------------------

input = raw_input("URL: ")
response = fetch(input)
html = parseHtml(response)
writeFile(html)
