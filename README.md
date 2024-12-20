ArgoCD Notifications TimeZone Güncellenmesi

Sorun ve Özet:

Openshift GitOps operatörü kurulduktan sonra ArgoCD instance yaratılmakta ve ArgoCD create, deploy, sync vb. olayları mail, webhook, slack gibi kanallarla notifikasyon bildirimleri yapılabilmektedir.

Bilindiği üzere Openshift GitOps operatörü ile yaratılan instances operatör tarafından yönetilmektedir.

Notifikasyon template,servis,trigger ve subscriptionlar “NotificationsConfigurations” instance üzerinde yapılan değişiklik ile yapılmakta ve “argocd-notifications-cm” güncellenmektedir.

Default olarak notifikasyonlar “ZULU” time zone da gönderilmektedir.

Çözüm:

Bulunulan bölgeye uygun timezone bilgisini içeren bir ConfigMap openshift-gitops projesi altında yaratılır.

“openshift-gitops-notifications-controller” deploymenta yaratılan config map environment olarak eklenir.

Notifikasyon templatelerinde ilgili time alanı düzenlenir.

Örn: {{.app.status.changedAt | date “2006-02-01 15:04:05”}}

openshift-gitops-notifications-controller podları restart edilir.
