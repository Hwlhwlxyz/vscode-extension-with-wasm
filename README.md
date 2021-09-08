# vscode-extension-with-wasm
## rust部分

安装 cargo install wasm-pack

```bash
cargo install wasm-pack
```

```bash
cargo new --lib wasm-lib
```

src中代码

```rust
use wasm_bindgen::prelude::*;

#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}

#[wasm_bindgen]
pub fn add(a: u32, b: u32) -> u32 {
    a + b
}
```

Cargo.toml

```rust
[package]
name = "wasm-lib"
version = "0.1.0"
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2"
```

编译

```bash
wasm-pack build --target nodejs
```

进入pkg目录，运行命令

```rust
cd pkg
npm link
```

显示

```rust
npm notice created a lockfile as package-lock.json. You should commit this file.
```

## vscode 插件部分

```rust
yo code
```

extension-with-wasm

进入目录

运行link命令，和安装依赖

```rust
npm link wasm-lib
npm install
```

在extension中导入wasm-lib

```tsx
import * as wasm from 'wasm-lib';
```

extension.ts

```tsx

import * as vscode from 'vscode';
import * as wasm from 'wasm-lib';
export function activate(context: vscode.ExtensionContext) {
	console.log('Congratulations, your extension "extension-with-wasm" is now active!');
	let disposable = vscode.commands.registerCommand('extension-with-wasm.helloWorld', () => {
		vscode.window.showInformationMessage('Hello World from extension-with-wasm! ', String(wasm.add(1,2)));
	});
	context.subscriptions.push(disposable);
}
export function deactivate() {}
```
