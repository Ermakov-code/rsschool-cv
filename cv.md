1. Dmitry Svetilov
2. email: medison2033@gmail.com
3. Summary (your goal, wishes, reveal what is important for you, what do you want and why.
Some kind of self-presentation. In case of lack of experience  Junior Developer sells his/her potential, his/her passion and ability to learn fast. You shouldn't think that everybody is going to teach you when you come to the workplace . Rather being a Junior means always
learning new things from everywhere etc.).
4. Skills C++ leaner, some python and unity
5. Python: 
 import multiprocessing
import socket



node = {}

class WordAndStr():
    def __init__(self, word, strnum):
        self.word = word
        self.strnum = strnum
    def view(self):
        return "Word: " + self.word + "\nNum of str: " + self.strnum


def handle(connection, address):
    import logging
    logging.basicConfig(level=logging.DEBUG)
    logger = logging.getLogger("process-%r" % (address,))
    try:
        logger.debug("Connected %r at %r", connection, address)
        while True:
            data = connection.recv(1024).decode()

            if data.__eq__('view'):
                connection.sendall(b'Enter word to view: ')
                word1 = connection.recv(1024)
                if str(word1.decode()) in node:
                    connection.send(node[word1.decode()].view().encode())
                else:
                    print('false')
                    connection.send(b'false')
            elif data.__eq__('create'):
                connection.send(b'Enter word: ')
                word = connection.recv(1024).decode()
                connection.send(b'Enter str: ')
                strnumber = connection.recv(1024).decode()
                node[str(word)] = WordAndStr(word, strnumber)
                connection.send(b'done')
                logger.debug("Done with creating")
            elif data.__eq__('exit'):
                logger.debug("Socket closed remotely")
                connection.send(b'exit')
                break
           # logger.debug("Received data %r", data)
           # connection.sendall(data.encode())
           # logger.debug("Sent data")
    except:
        logger.exception("Problem handling request")
    finally:
        logger.debug("Closing socket")
        connection.close()

class Server(object):
    def __init__(self, hostname, port):
        import logging
        self.logger = logging.getLogger("server")
        self.hostname = hostname
        self.port = port

    def start(self):
        self.logger.debug("listening")
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.socket.bind((self.hostname, self.port))
        self.socket.listen(1)

        while True:
            conn, address = self.socket.accept()
            self.logger.debug("Got connection")
            process = multiprocessing.Process(target=handle, args=(conn, address))
            process.daemon = True
            process.start()
            self.logger.debug("Started process %r", process)

if __name__ == "__main__":
    import logging
    logging.basicConfig(level=logging.DEBUG)
    server = Server("localhost", 9000)
    try:
        logging.info("Listening")
        server.start()
    except:
        logging.exception("Unexpected exception")
    finally:
        logging.info("Shutting down")
        for process in multiprocessing.active_children():
            logging.info("Shutting down process %r", process)
            process.terminate()
            process.join()
    logging.info("All done")
6. Experience (for a Junior Dev it means all kinds of experience: coding tests, projects from courses,
freelance projects - wherever they had the opportunity to demonstrate skills they have.
Also it would be awesome if you add links to source code)
7. Education (including courses, seminars, lectures, online learning)
8. English (elaborate on what kind of practice you had, if any, how long it lasted and so on)