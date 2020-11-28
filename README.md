# nest-microservices-storage
- A NestJS module for `generator-nest-microservices` yeoman template for redis features
### How to use
- install package with `npm i -S nest-microservices-storage`
- Register the `StorageModule` to your feature modules:
```
@Module({})
export class AppModule {
  static register(config: ConfigOptions): DynamicModule {
    return {
      module: AppModule,
      imports: [
        StorageModule.register(config.storageOptions),
        ...
      ],
    };
  }
}
```
- Once `RedisModule` is registered, then you can use `StorageService` to upload files to Azure Storage
```
export class ExampleController {
  constructor (private service: StorageService) {
    super (service);
  }

  @Post('upload')
  @UseInterceptors(FileInterceptor('file'))
  async upload(@UploadedFile() file: any): Promise<string> {
    const url = await this.service.uploadFile(file, 'container-name', 'custom/path/here');
    return url;
  }
}
```
### Storage options
|Property|Type|Required|Description|
|-|-|-|-|
|connectionString|`String`|true| Connection string for your Azure Storage
|accessKey|`String`|true| Access key for your Azure Storage
|accountName|`String`|true| Account name of your Azure Storage
|containerName|`String`|true| Default container to upload files to
