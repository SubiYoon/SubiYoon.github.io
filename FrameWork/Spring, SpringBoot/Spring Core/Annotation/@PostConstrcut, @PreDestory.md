---
tags:
  - Spring
  - Core
  - Annotation
Reference link: "[[@Configuration, @Bean, @Component]]"
---
### @PostConstruct
자바 구성(`@Configuration`) Class 외부에 POJO Class를 정의(`@Component`) 할 경우 사용합니다. `@Bean(initMethod="${methodName}"`와 같은 동작을 하게 됩니다.

### @PreDestory
자바 구성(`@Configuration`) Class 외부에 POJO Class를 정의(`@Component`) 할 경우 사용합니다. `@Bean(destoryMethod="${methodName}"`와 같은 동작을 하게 됩니다.

```java title:"Cashier.java"
@Component
public Class Cashier {

	@Value("checkout")
	private String fileName;
	@Value("HOME/user/Temp/cashier")
	private String path;
	private BufferedWriter writer;

	public void setFileName(String fileName) {
		this.fileName = fileName;
	}

	public void setPath(String path) {
		this.path = path;
	}

	@PostConstruct
	public void openFile() throws IOException {
		File targetDir = new File(path);

		if(!targetDir.exists()) {
			targetDir.mkdir();
		}

		File checkoutFile = new File(path, fileName + ".txt");

		if(!checkoutFile.exists()) {
			checkoutFile.createNewFile();
		}

		writer = new BufferedWriter(new OutputStreamWiter(new FileOutputStream(checkoutFile, true)));
	}

	public void checkout(ShoppingCart cart) throws IOEception {
		writer.write(new Date() + "\t" + cart.getItems() + "\r\n");
		writer.flush();
	}

	@PreDestory
	public void closeFile() throws IOEception {
		writer.close();
	}
}
```