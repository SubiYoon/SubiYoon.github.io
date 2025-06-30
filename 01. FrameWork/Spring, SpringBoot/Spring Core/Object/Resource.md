---
tags:
  - Spring
  - Core
  - Object
---
## Resource
Spring의 `Resource`객체를 활용하면 다른 정적인 파일들도 읽어 올 수 있습니다.

```java title:"BannerLoader.java"
public class BannerLoader {

	private Reosurce banner;

	public void setBanner(Resource banner) {
		this.banner = banner;
	}

	@PostContstruct
	public void showBanner() throws IOException {
		Files.lines(Path.get(banner.getURI()), Charset.forName("UTF-8"))
			.forEachOrdered(System.out::println);
	}
}
```

showBanner() 메서드로 인해 배너 파일의 내용을 차례때로 읽어 콘솔에 한 줄씩 출력할 것입니다. `@PostConstruct`를 붙여 Spring에게 빈을 생성한 후 이 메서드를 자동으로 실행하도록 지시합니다. 아래 코드를 추가하여 로직을 완성하겠습니다.

```java title:"ShopConfiguration.java"
@Configuration
@PropertySource("classpath:discounts.properties")
@ComponentScan("com.apress.springrecipes.shop")
public class ShopConfiguration {

	@Value("${classpath:banner.txt}")
	private Resource banner;

	@Bean
	public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer() {
		return new PropertySourcesPlaceholderConfigurer();
	}

	@Bean
	public BannerLoader bannerLoader() {
		BannerLoader bl = new BannerLoader();
		bl.setBanner(banner);
		return bl;
	}
}
```


`classpath`가 아닌 절대경로, URL로 위치를 특정할 수도 있습니다. 

```java
Resource resource1 = new FileSystemResource("c:/shop/banner.txt");
Rexource resource2 = new UrlResource("http://www.apress.com/");
```