# Install

### 1. Git clone this repo.

### 2. Drag iosSDK.xcodeproj file to your project.

![1](https://github.com/nebulasio/iosSDK/blob/master/1.png)

### 3. Add link library.

![2](https://github.com/nebulasio/iosSDK/blob/master/2.png)

### 4. Add header search path by dragging iosSDK folder to the setting area.

![3](https://github.com/nebulasio/iosSDK/blob/master/3.png)

# Usage

## Wallet

Import NASSmartContracts.h file.

```obj-c
#import <NASSmartContracts.h>
```

### 1. Debug mode

```obj-c
[NASSmartContracts debug:YES]; // use the debug net
```

### 2. Check wallet APP

```obj-c
if (![NASSmartContracts nasNanoInstalled]) {
    // if wallet APP is not installed, go to AppStore.
    [NASSmartContracts goToNasNanoAppStore];
}
```

### 3. Pay

```obj-c
    self.sn = [NASSmartContracts randomCodeWithLength:32];
    NSError *error = [NASSmartContracts payNas:@(0.0001)
                                     toAddress:@"n1a4MqSPPND7d1UoYk32jXqKb5m5s3AN6wB"
                              withSerialNumber:self.sn
                                  forGoodsName:@"test1"
                                       andDesc:@"desc"];
```

### 4. Call contract function

```obj-c
    self.sn = [NASSmartContracts randomCodeWithLength:32];
    NSError *error = [NASSmartContracts callWithMethod:@"save"
                                               andArgs:@[@"key111", @"value111"]
                                                payNas:@(0)
                                             toAddress:@"n1zVUmH3BBebksT4LD5gMiWgNU9q3AMj3se"
                                      withSerialNumber:self.sn
                                          forGoodsName:@"test2"
                                               andDesc:@"desc2"];
```

### 5. Check status

```obj-c
    [NASSmartContracts checkStatusWithSerialNumber:self.sn
                             withCompletionHandler:^(NSDictionary *data) {
                                 NSData *json = [NSJSONSerialization dataWithJSONObject:data options:NSJSONWritingPrettyPrinted error:nil];
                                 NSString *string = [[NSString alloc] initWithData:json encoding:NSUTF8StringEncoding];
                                 NSLog(@"%@", string);
                             } errorHandler:^(NSInteger code, NSString *msg) {
                                 NSLog(@"%@", msg);
                             }];
```

## API

Import NASApi.h file.

```obj-c
#import <NASApi.h>
```

### 1. Debug mode

Debug mode will connect to the debug net.

```obj-c
[NASApi debug:YES]; // use the debug net
```

### 2. call API

See the [API document](https://github.com/nebulasio/wiki/blob/e340e736c6756d54bca7a655e432db9f9a257544/rpc.md).

We do not define model for response and return a dictionary instead. You can use your favorite json library to parse the data.

```
completionHandler:(void (^)(NSDictionary *data))handler
errorHandler:(void (^)(NSString *msg))errorHandler
```

### 3. Contract

You can form a contract by calling `contractWithSource:andSourceType:andFunction:andArgs`.

```
    [NASApi contractWithSource:nil
                 andSourceType:nil
                   andFunction:@"transferValue"
                       andArgs:[@(500)]];

```
