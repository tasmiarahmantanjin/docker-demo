FROM node

WORKDIR /client

COPY package*.json /client/

RUN npm install

COPY . /client/

EXPOSE 3000

CMD ["npm", "start"]

# How to Build: docker build -t my-github-finder .
# How to Run: docker run -p 3000:3000 my-github-finder