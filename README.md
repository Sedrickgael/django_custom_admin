# django_custom_admin
personnaliser l'admin de Django


```bash
from django.contrib import admin

#importation des models
from . import models

#importations des models d'une autre application
from application import models as application_models

#register models

@admin.register(models.Nom_du_model)
#ou
@admin.register(application_models.Nom_du_model)

#création de la class
class Nom_modelAdmin(admin.ModelAdmin):
#exemple ChoiceAdmin(admin.ModelAdmin):

#sur l'admin de django, une après avoir cliquer sur un model, nous sommes redirigés sur un page où les données s'affichent sous forme de tableau.  les champs du tableau sont les champs mis dans list_filter. on peut mettre dans list_filter: les charField, le foreignkey, les standars (status, date_add) , les images (placer en debut de liste s'il n'y a pas de foreignkey, et placer à la fin si oyui) 
  list_display = ('',)
  
 # Dans l'admin, après avoir cliquer sur un model, nous avons la possibilité de faire des tries, les champs utilisés pour les tries doivent être placer dans list_filter, on peut y mettre en premier les standards, ensuite les foreignkey ou many2many
  
  list_filter = ('',)
  
 # Le search_fields corespond au champ de recherche, l'administrateur peur faire des recherches dans les données avec. on ajoute le search_fields le champs aillant été mis dans display_list et de préférence les champs comme le nom, le titre
  search_fields = ('',)
  
# date_hierarchy nous permet d'empiler les données par jour, mois, années. ainsi il est plus facile d'accéder au enregistrement d'une journée, d'un mois, d'une années précise. 
  date_hierarchy = 'created_at'
  
 # Dans l'admi il est possible d'éffectuer des actions groupées, par défaut nous avons le delete. pour ajouter une action procéder comme suit. dans l'exemple ci-dessous nous avons ajouté les actions activé et désactive qui permettent repectivement d'activé et desactivé une selection 
  
  actions = ('active','deactive')
  def active(self, request, queryset):
      queryset.update(produit_collections_is_view=False)
      self.message_user(request, 'La selection a été deactivé avec succès')

  active.short_description = 'Desactiver'

  def deactive(self, request, queryset):
      queryset.update(produit_collections_is_view=True)
      self.message_user(request, 'La selection a été activé avec succès')

  deactive.short_description = 'Activer'
  
# Ici il s'agit du champ permettant d'acceder aux information d'un enregistrement donné 
  list_display_links = ['sku' ,'title',]
  
# Ordering est comme un trie du tableau mais pas comme les list_filter, ici tous les données sont affichés soit par ordre alphabétique ou par ordre croissant qui est l'ordre d'afffichage par défaut.
  ordering = ['',]
  
 # Ici il s'agit du nombre d'enregistrement à afficher par page '''
  list_per_page = 20
  fields = ['',]
  readonly_fields = ['']

#pour les enregistrment (ajouter un élément), pour les champs many2many, il faut les placer dans filter_horizontal afin qu'il s'affiche #normalement 
  filter_horizontal = ('')

  
  
#lorsqu'un model à des liés à un autre model, pour pouvoir l'enregistrer celui qui est lié à l'autre, il faut ajouter dans inlines le #nom de la class qui herite du admin.TabularInline (voir en bas) et qui prend en model le model qui prend la liaison, ainsi étant dans #le premier modele, on peut ajouter des élements pour le secon modele qui prend la liaison
  inlines = [
      FichierInLine
  ]

# fildsets nous permet de grouper les champs pour les affichés sous forme de session lors de l'enregistrement 
  fieldsets = [
          ('Nom', {'fields': ['repectoire', 'role']})
          ],


  

  #cette fonction nous pemmet d'avoir une vue d'image
  def view_image(self, obj):
      return mark_safe('<img src="{url}" width="100px" height="100px" />'.format(url = obj.look_image))




class ModelInLine(admin.TabularInline):
    model = models.Model
    extra = 0



def _register(model, admin_class):
    admin.site.register(model, admin_class)


_register(models.Model, ModelAdmin)
```
