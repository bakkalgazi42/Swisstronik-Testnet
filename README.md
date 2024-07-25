<h1 align="center">Swisstronik-Testnet DEPLOY A SIMPLE CONTRACT USING HARDHAT</h1>

> UYARI : Contract Deploy ettiğimiz için herhangi bir sunucuda kurabilirsiniz  Npm ve Nodejs paketlerinin güncel olmasına dikkat edin

#

<h1 align="center">Donanım</h1>

```
herhangi bir sunucunuzda kurabilirsiniz.
```
<h1 align="center">Kurulum</h1>

```console
# dizin ve dosya  oluşturma
cd $HOME
mkdir Swiss

# Nodejs ve npm kurulumu
sudo apt-get remove -y nodejs
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash && export NVM_DIR="/usr/local/share/nvm"; [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"; [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"; source ~/.bashrc; nvm install --lts; nvm use --lts

# Hardhat kurulumu
npm install --save-dev hardhat
npm install --save-dev @nomicfoundation/hardhat-toolbox
npx hardhat

# gelen sorulara Enter ve Yes diyerek ilerleyin..

```

```console
> İçerisine gerekli değişkenleri giriyoruz ardından ctrl+x, y ve enter tuşlarına basarak çıkıyoruz.

rm hardhat.config.js
nano hardhat.config.js


# buradaki kod bloğunda sadece 0xd5 ile başlayan kelimenin olduğu yere 0x kalmmak suretiyle mm cüzdanın privatekeyini giriyoruz
require("@nomicfoundation/hardhat-toolbox");

module.exports = {
  solidity: "0.8.19",
  networks: {
    swisstronik: {
      url: "https://json-rpc.testnet.swisstronik.com/",
      accounts: ["0xPRİVATE_KEY"], //Your private key starting with "0x"
    },
  },
};


```

```console
# Contract deploy etmek için ilgili dizine girip nano dosyasını düzenliyoruz.
cd contracts
rm Lock.sol
nano Lock.sol
```

```console
# Bu kod blogunu herhangi bir değişiklik yapmadan nano dosyasına yapıştırın ve ctrl+x,y,enter deyip çıkıyoruz.
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.19;

contract Swisstronik {
    string private message;

    constructor(string memory _message) payable{
        message = _message;
    }

    function setMessage(string memory _message) public {
        message = _message;
    }

    function getMessage() public view returns(string memory){
        return message;
    }
}
```

```console
# Bu komutla hardhat dosyasını complie ediyoruz.
npx hardhat compile
```

> Script dosyasının sağlıklı çalışabilmesi için gerekli dosyaları düzenleyip oluşturuyoruz.

```console
cd ..
mkdir scripts && cd scripts
nano deploy.js
```

> Deploy config İçerisine gerekli değişkenleri giriyoruz ardından ctrl+x, y ve enter tuşlarına basarak çıkıyoruz.

```console
# buradaki kod bloğunda değiştirmeniz bir yer yok.
const hre = require("hardhat");

async function main() {

  const contract = await hre.ethers.deployContract("Swisstronik", ["Hello Swisstronik!!"]);

  await contract.waitForDeployment();

  console.log(`Swisstronik contract deployed to ${contract.target}`);
}
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

> Daha sonra ana dizine gelip scriptimizi çalıştırıyoruz

```console
cd
cd Swiss
npx hardhat run scripts/deploy.js --network swisstronik
```
# Başarılı bir deploy yaptıktan sonra terminal ekranında "Swisstronik contract deployed to 0x..." çıktısını görürseniz başarılı olmuşsunuzdur demektir. Ve oluşturduğunuz Contract adresini bir yere not edin lazım olacak :)

```console
# Biz her ihtimale karşı içeriğinde bulunan PRİVATE_KEY i silelim ve değişiklikleri yaptıktan sonra ctrl+x, y, enter diyerek çıkalım.
nano hardhat.config.js
```


> Sonraki aşamada ise görevde belirlenen github reposu oluşturma için github reposu oluşturacağız..
> önce güncellemeleri yapalım
```console
sudo apt update
sudo apt install git
```
> github düzenlemeleri yapıyoruz
```console
git config --global user.name "Your Name"
git config --global user.email "youremail@gmail.com"
git init
git add .
git commit -m "Initial commit"
```
> sonrasında Github üzerinde manuel bir repo oluşturuyoruz ve resimdeki yerden repomuzu oluşturup linki alıyoruz ve aşağıdaki koda yazıyoruz
![1](https://github.com/user-attachments/assets/b9482343-9890-4b31-917a-5f3bbecdfaff)
![2](https://github.com/user-attachments/assets/8b295712-2f24-4a83-8867-8ac36ad63aaa)

```console
git remote add origin GİTHUB_REPO_LİNKİ
```
> Aşağıda yapmamız gereken işlemi yalnız birkez yapacağız ve repoda yaptığımız işlemleri yukarıda oluşturduğumuz repoya çekmek için oluşturacağımız GENERATE_KEY i bir yere not edin.
> Şimdi [buraya](https://github.com/settings/tokens/new) gidiyoruz ve eğerki şifre isterse şifre giriyoruz
> `Note` kısmına istediğinizi yazabilirsiniz.
> `Select scopes` kısmını ise `repo` kısmını seçerek GENERATE TOKEN'e tıklayın.
> Oluşturmuş olduğumuz KODU githuba veri çekmek için PASSWORD olarak kullanacağız.
> 
# Repodaki Main dizinine git işlemleri yapıyoruz.
```console
git branch -M main
```
```console
git push -u origin main
```
> Daha önce almış olduğumuz kullanıcı adı ve GENERATE TOKEN ile son komutta giriyoruz ve işlemlerimiz burada bitiyor
> [Dashboard](https://github.com/settings/tokens/new) sayfasına giderek Linki ve Yukarıda Oluşturmuş olduğumuz Contract adresini girerek 1.görevi tamamlıyoruz.






