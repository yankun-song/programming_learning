# Template
```js
const http = require('http');
const fs = require('fs');
const path = require('path');

const server = http.createServer((request, response)=>{
    if (request.method == 'GET' && request.url === '/'){
        response.writeHead(200, { 'Content-Type': 'text/html' });
        response.write(fs.readFileSync(__dirname + '/index.txt'));
        response.end();
    }
    else if (request.method == 'POST' && request.url === 'greeting'){
        let body = '';
        request.on('data', chunk => {
            console.log(`Received data chunk: ${chunk}`);
            body += chunk;
        });
        request.on('end', () => {
            fs.appendFileSync('./hi_log.txt', `${body}\n`);
            respond.end('good morning');
        })
    }
    else{
        response.writeHead(404);
        return response.end('Error: not found');
    }
}).listen(3000, () => console.log('listening on 3000'));
```