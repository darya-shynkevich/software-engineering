To expose the functionality of our service, we can use REST APIs for the following features:
- Shortening a URL
- Redirecting a short URL
- Deleting a short URL

! Only leader-followers replication because we do not want to solve conflicts

? We can generate [ULIDs](https://victoryosayi.medium.com/ulid-universally-unique-lexicographically-sortable-identifier-d75c253bc6a8) and use Bloom Filer to check if this ULID is free or not.

? Maybe we can use Redis as our primary DB to store key=long_url, value=short_url

! We can use Redis to cache popular urls

![](../../../_Attachments/Pasted%20image%2020240128144745.png)


# References:

1. ! [The URL Shorteners Design and Implementation](https://jinlow.medium.com/the-url-shorteners-design-and-implementation-edd4e55c8d65)
2. [URL Shortening System Design](https://systemdesign.one/url-shortening-system-design/)
3. [Designing a URL Shortening service like TinyURL](https://medium.com/@sandeep4.verma/system-design-scalable-url-shortener-service-like-tinyurl-106f30f23a82)
4. [Tiny URL System Design](https://nikhilgupta1.medium.com/tiny-url-system-design-846a66c7f9d3)
5. [System Design: URL Shortener](https://medium.com/@the.york.wei/system-design-url-shortener-1de7acb82612)
6. [URL System Design](https://medium.com/@karan99/system-design-url-shortener-c7bd9e35a0f1)
7. [URL shortener system design | tinyurl system design](https://www.youtube.com/watch?v=JQDHz72OA3c&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=4) (video)
8. [URL Shortener System Design (Explained with Two proposed Designs with Pro and Cons of each)](https://www.youtube.com/watch?v=5uX3rDRaHXw&list=PLQnljOFTspQXSevtRqvMNycWfHM7cXc3d&index=1)
9. [System Design — URL Shortening Service](https://medium.com/@anuupadhyay1994/system-design-url-shortening-service-7df292a44f09)
10. [**Пишем сервис для сокращения ссылок на Django, DRF**](https://habr.com/ru/articles/718800/)
11. **[Build a URL Shortener With FastAPI and Python](https://realpython.com/build-a-python-url-shortener-with-fastapi/)**
12. [System Design for Big Data-tinyurl](http://n00tc0d3r.blogspot.com/)
13. [Tiny URL System Design](https://www.youtube.com/watch?v=AVztRY77xxA) (video)
14. [Microsoft System Design Interview Question | Design A URL Shortener](https://www.youtube.com/watch?v=1uEN6zbmmO0&list=PLOAph0xkZvSuqy8yq_0D6NEABhmSTRYrN&index=1) (video)
15. [URL Shortening System Design](https://medium.com/nerd-for-tech/url-shortening-system-design-88cd005604b4) + [Knowledge Test](https://medium.com/nerd-for-tech/knowledge-test-url-shortening-system-design-ad80b7e3073f)
16. [Design URL Shortener](https://medium.com/@samjingwen/design-url-shortener-de7214ecea3)
17. [URL shortener System design](https://medium.com/@narengowda/url-shortener-system-design-3db520939a1c)