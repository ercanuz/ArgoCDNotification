ArgoCD Notifications TimeZone Güncellenmesi

Sorun ve Özet:

1. Openshift GitOps operatörü kurulduktan sonra ArgoCD instance yaratılmakta ve ArgoCD create, deploy, sync vb. olayları mail, webhook, slack gibi kanallarla notifikasyon bildirimleri yapılabilmektedir.
   
3. Bilindiği üzere Openshift GitOps operatörü ile yaratılan instances operatör tarafından yönetilmektedir.
   
5. Notifikasyon template,servis,trigger ve subscriptionlar "NotificationsConfigurations" instance üzerinde yapılan değişiklik ile yapılmakta ve "argocd-notifications-cm" güncellenmektedir.
   
7. Default olarak notifikasyonlar "ZULU" time zone da gönderilmektedir. 

Çözüm:

1. Bulunulan bölgeye uygun timezone bilgisini içeren bir [ConfigMap] openshift-gitops projesi altında yaratılır.(https://github.com/ercanuz/ArgoCDNotification/blob/main/timezone-config-map.yaml) yaratılır.

2. "openshift-gitops-notifications-controller" deploymenta yaratılan config map [environment](https://github.com/ercanuz/ArgoCDNotification/blob/main/argocd-notification-controller-deployment.jpg) olarak eklenir.
   
3. Notifikasyon templatelerinde ilgili time alanı düzenlenir.

Örn: {{.app.status.changedAt | date "2006-02-01 15:04:05"}}
    
4. openshift-gitops-notifications-controller podları restart edilir.
