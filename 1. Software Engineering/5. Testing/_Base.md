
[localstack](https://www.localstack.cloud/) - develop and test your AWS applications locally to reduce development time and increase product velocity. Reduce unnecessary AWS spend and remove the complexity and risk of maintaining AWS dev accounts

# Books:

1. [Хориков В. - Принципы юнит-тестирования](http://libgen.rs/book/index.php?md5=FFFC58A9B1793A61F7EFF6EFCBDA11F2). Рекомендую, есть как на русском, так и на англ. Тема сама по себе не очень обширная (хотя в книге и интегр тестирование рассматривается), но рассмотрена со всех возможных сторон. Изложение слегка хромает, при этом книжка очень практическая, чувствуется, что автор сформировал всё на собственном обширном опыте. Есть спорные моменты, однако бескомпромиссных решений не бывает.
2. **[Testing Distributed Systems w/ Deterministic Simulation](https://www.youtube.com/watch?v=4fFDFbi3toc)** (video)
3. **[Deterministic Simulation: A New Era of Distributed System Testing](https://www.risingwave.com/blog/deterministic-simulation/)**
4. [Cloud Chaos & Microservices Mayhem](https://www.youtube.com/watch?v=zJiUNKXZA1E) (video) Many organisations adopt microservices to try and ensure they’re ‘doing cloud right’, but microservices are an architectural solution to an operational, social, and organisational challenge. On their own, microservices do not ensure architectural decoupling, and they can introduce considerable technical risk. What’s the solution? Contract tests. They’re not the full solution (remember, this is an organisational challenge), but contract tests can be a great tool for reducing the risk of microservices deployments.
5. [Python Testing with pytest](http://libgen.rs/book/index.php?md5=704A51F1CAE3EB7BD6ABC0C954927079) (book)
6. [Testing in Python](http://libgen.rs/book/index.php?md5=2732228E2680E2349A93F06D43F0692D) (book)

# References:

*[Awesome pytest](https://github.com/sobolevn/awesome-pytest) (number of awesome articles)*

1. ! [System Quality — Testing Pyramid](https://jinlow.medium.com/system-quality-testing-pyramid-0f9436fecf23)
2. ! [How Much Testing is Enough?](https://testing.googleblog.com/2021/06/how-much-testing-is-enough.html "How Much Testing is Enough?")
3. ! [Advanced pytest techniques I learned while contributing to pandas](https://levelup.gitconnected.com/advanced-pytest-techniques-i-learned-while-contributing-to-pandas-7ba1465b65eb)
4. [Be the idiot](https://luminousmen.com/post/be-the-idiot)
5. [Testing in Production: the hard parts](https://copyconstruct.medium.com/testing-in-production-the-hard-parts-3f06cefaf592)
6. [Введение в тестирование в Python](https://webdevblog.ru/vvedenie-v-testirovanie-v-python/)
7. [Простые рецепты хороших юнит тестов](https://www.youtube.com/watch?v=ZyGZjpxF9Fo) (video)
8. [Тесты в Python: все основные подходы, плюсы и минусы. Доклад Яндекса](https://habr.com/ru/company/yandex/blog/517266/)
9. [Building Good Tests](https://salmonmode.github.io/2019/03/29/building-good-tests.html)
10. [Матвей Коняев, Точка. Как писать тесты дешевле](https://www.youtube.com/watch?v=MJPZ-XrWLsw) (video)
11. [Pytest-фикстуры на человеческом](https://habr.com/ru/articles/716248/)
12. [Python Testing с pytest. ГЛАВА 3 pytest Fixtures](https://habr.com/ru/articles/448786/)
13. [Python Testing с pytest. Builtin Fixtures, Глава 4](https://habr.com/ru/articles/448792/)
14. [Транзакционное юнит-тестирование приложений с БД](https://habr.com/ru/company/selectel/blog/598499/)
15. [Пишем и тестируем миграции БД с Alembic](https://habr.com/ru/company/yandex/blog/511892/)
16. [Тестируем слой БД в Python с использованием pytest и testcontainers](https://habr.com/ru/articles/677598/)
17. [How to simulate a broken database connection for testing in Django](https://neilkakkar.com/test-database-connection-django.html)

#### Hypothesis:

1. [Python Testing with doctest](https://www.blog.pythonlibrary.org/2014/03/17/python-testing-with-doctest/)
2. [Изучаем mutmut — инструмент для мутационного тестирования на Python](https://habr.com/ru/company/vdsina/blog/512630/)
3. [Getting Started With Property-Based Testing in Python With Hypothesis and Pytest](https://semaphoreci.com/blog/property-based-testing-python-hypothesis-pytest)
4. [Welcome to Hypothesis!](https://hypothesis.readthedocs.io/en/latest/)

## Test performance:

1. [13 Proven Ways To Improve Test Runtime With Pytest](https://pytest-with-eric.com/pytest-advanced/pytest-improve-runtime/#Identify-The-Problem-Tests)
2. [How we run our Python tests in hundreds of environments really fast](https://www.google.com/url?q=https://blog.sentry.io/2022/11/14/how-we-run-our-python-tests-in-hundreds-of-environments-really-fast/&sa=D&source=calendar&usd=2&usg=AOvVaw25M9U-3atwnetPRatCIAFU)
3. [Effective Test Automation Approaches for Modern CI/CD Pipelines](https://www.infoq.com/articles/test-automation-ci-cd/)
4. [Improve pytest performance](https://discord.com/blog/pytest-daemon-10x-local-test-iteration-speed?ref=architecturenotes.co)
5. [Effective Python Testing With Pytest](https://realpython.com/pytest-python-testing/)
6. [Ускоряем тестирование: быстро, модно, без рутины](https://habr.com/ru/companies/qiwi/articles/702302/)
7. [How to write tests that need a lot of data?](https://gogognome.nl/how-to-write-tests-that-need-a-lot-of-data.html)

### TDD

1. [My 19 Laws of Test Driven Development](https://www.youtube.com/watch?v=q9wgjfeloc4&list=PLcTa2e7_ENN_ZmTmGC_AFh1ArFgdEb5Z6&index=44) (video)
2. [TDD in Python with pytest](https://www.thedigitalcatonline.com/blog/2020/09/10/tdd-in-python-with-pytest-part-1/) (several articles)
3. [Build a Hash Table in Python With TDD](https://realpython.com/python-hash-table/)
4. [Rumbling about Test Driven Development](https://luminousmen.com/post/rumbling-about-test-driven-development)