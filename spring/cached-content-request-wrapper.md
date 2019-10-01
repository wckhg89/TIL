# CachedContentRequestWrapper

ServletRequest 의 리퀘스트 바디를 읽으면 InputStream을 재사용할 수 없어서, 한번 wrapping 한 클래스

사용한 InputStream은  ``ByteArrayOutputStream cachedContent`` 에 저장된다.
따라서, 리퀘스트 바디를 재사용하려면 cachedContent를 resturn 해주는 ``getContentAsByteArray`` 메소드를 이용해야한다.


cf)
https://www.sollabs.tech/ContentCachingRequestWrapper
https://meetup.toast.com/posts/44
