Docker Desktop'ı Windows üzerinde kurmak için şu adımları takip edebilirsiniz:

1. **Docker Desktop'u İndirin:**
   [Docker Desktop indirme sayfasına](https://www.docker.com/products/docker-desktop) gidin ve "Download for Windows" (Windows İçin İndir) düğmesine tıklayın.

2. **Docker Desktop'u Kurun:**
   İndirdiğiniz kurulum dosyasını çalıştırın (genellikle `Docker Desktop Installer.exe` olarak adlandırılır) ve kurulum sihirbazını takip edin. Sihirbaz sizi kurulum sürecinde yönlendirecektir.

3. **Hyper-V'yi Etkinleştirin (Windows 10 Pro, Enterprise veya Education için):**
   Docker Desktop, Hyper-V'nin etkin olmasını gerektirir. Eğer Windows sürümünüz belirtilen sürümlerden biriyse, kurulum sırasında Hyper-V'yi etkinleştirmeniz istenecektir. Talimatları takip ederek etkinleştirin.

4. **Docker Desktop'u Yapılandırın:**
   Kurulumdan sonra, Docker Desktop sisteminizde bildirim alanında görünecektir. Docker simgesine sağ tıklayın ve "Ayarlar" seçeneğini seçin. Burada, kaynak tahsisi, ağ vb. gibi çeşitli ayarları yapılandırabilirsiniz.

5. **Docker Desktop'u Başlatın:**
   Docker Desktop'ı Başlat menüsünden veya masaüstü kısayolundan açın. Başlaması birkaç dakika sürebilir ve hazır olduğunda sistem tepsisindeki Docker simgesi yeşile dönecektir.

6. **Kurulumu Doğrulayın:**
   PowerShell veya Komut İstemi'ni açın ve Docker'ın kurulu ve düzgün çalıştığından emin olmak için aşağıdaki komutları çalıştırın:
   ```powershell
   docker --version
   docker run hello-world
   ```

   `docker run hello-world` komutu küçük bir test imajını indirip çalıştıracaktır. Her şey düzgün bir şekilde kurulmuşsa, Docker kurulumunuzun çalıştığını onaylayan bir mesaj görmelisiniz.

İşte bu kadar! Windows makinenize Docker'ı başarıyla kurmuş oldunuz. Unutmayın ki Docker Desktop'ın tam işlevselliği için Hyper-V gereksinimi nedeniyle Windows 10 Pro, Enterprise veya Education sürümleri gereklidir. Farklı bir sürüm kullanıyorsanız Docker Toolbox kullanmanız veya Windows sürümünüzü yükseltmeniz gerekebilir.

Her zaman en güncel ve doğru bilgiler için resmi Docker belgelerine başvurun: [Docker Documentation](https://docs.docker.com/desktop/install/windows-install/).





Docker ve Git'i kurduktan sonra, bir sonraki adım, Label Studio ML Backend git deposunu sistemimize klonlamaktır.

bash
Copy code
git clone https://github.com/HumanSignal/label-studio-ml-backend.git
cd label-studio-ml-backend/label_studio_ml/examples/segment_anything_model
Sonra, Docker İmajını oluşturun.

bash
Copy code
docker build . -t sam:latest
Bu işlem, modelin Docker imajına gömülmesi için 2.4 GB boyutundaki SAM model ağırlıklarını içerir ve internet bağlantı hızınıza bağlı olarak 20 dakikaya kadar sürebilir. Bu yapı süreci, üretim kullanımı için model ağırlıklarını güncellemelere ve checkpoint'leme olanak tanımak amacıyla model çalışma zamanından ayrı bir şekilde saklamak en iyi uygulamadır.

Modelin oluşturulduğunu ve kullanıma hazır olduğunu doğrulayın.

bash
Copy code
docker image list
Docker, kullanılabilir imajların bir listesini çıkarmalıdır ve benzer bir giriş içeren bir girişle birlikte bir çıkış vermelidir:

Copy code
REPOSITORY         TAG              IMAGE ID       CREATED         SIZE
sam                latest           f69344cb96a5   5 minutes ago   4.61GB
SAM ML Backend'i kullanma

İmaj oluşturulduktan sonra, Label Studio kullanarak bir görüntü segmentasyon projesi oluşturma zamanı geldi.

Label Studio'yu yükleyin

Öncelikle, Label Studio'yu yüklemeniz gerekiyor. Bu örnekte, SAM ML Backend, yerel depolama sunumu etkinleştirilmiş olmalıdır. Bununla birlikte çalışan bir Label Studio örneği başlatmak için aşağıdaki komutu girin:

bash
Copy code
docker run -it -p 8080:8080 \
    -v $(pwd)/mydata:/label-studio/data \
    --env LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED=true \
    --env LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT=/label-studio/data/images \
    heartexlabs/label-studio:latest
Bu komut, Docker'ın Label Studio'yu başlatmasını, http://localhost:8080 adresinden görüntülenmesini, veritabanını ve görev dosyalarını yerel sabit sürücünüze depolamasını ve yerel dosya sunumunu etkinleştirmesini söyler. Label Studio başladığında, tarayıcınızla http://localhost:8080 adresine giderek Label Studio giriş ekranıyla karşılaşacaksınız.



