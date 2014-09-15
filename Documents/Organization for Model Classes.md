# Organization for Model Classes

The aim of this file is to show how to order model and controller files and classes to create a pattern of CODELAND. But why is important to set a standart for this kind of thing? **Simple, because when everybody *speak* the same language, everyone undertands each other among themselves. Patterns aren't for make everybody think equal, but to organize things, just that**, keep it in mind.

So, ok, said all this let's go to the CODE!!

First of all, seem that everyone have a tendency to write a model class with his own style, let's write a models class first!

## Organization of a Model Class

```
class Quote < ActiveRecord::Base
  self.inheritance_column = :role

  before_create :set_unique_id

  devise :invitable, :database_authenticatable, :recoverable,
         :rememberable, :trackable, :validatable, validate_on_invite: true

  belongs_to :destination_port, class_name: 'Port'
  belongs_to :origin_port, class_name: 'Port'
  has_many :quotation_proposals

  validates :product, :density, :msds, :destination_port, :origin_port, :quantity, presence: true
  validates :origin_pick_up, :origin_customs_clearance,
            :origin_loading_supervision, :destination_pick_up,
            :destination_customs_clearance, :heated,
            :destination_discharge_supervision, inclusion: { in: [true, false] }
  validates :origin_address, :origin_city, :origin_state, :origin_zip_code, :origin_country
            presence: true, if: :origin_pick_up?
  validates :destination_address, :destination_city, :destination_state, :destination_zip_code,
            :destination_country, presence: true, if: :destination_pick_up?

  validate :destination_port_must_be_different_from_origin

  has_enumeration_for :status
  has_enumeration_for :volume

  mount_uploader :msds, QuoteMdsdUploader

  def origin_pick_up?
    self.origin_pick_up == true
  end

  def destination_pick_up?
    self.destination_pick_up == true
  end

  protected

  def full_address(local = :destination)
    "#{send("#{local}_address")}, #{send("#{local}_city")} - #{send("#{local}_state")}
      CEP#{send("#{local}_zip_code")} - #{send("#{local}_country")}"
  end

  private

  def set_unique_id
    self.slug_id = loop do
      random_token = SecureRandom.urlsafe_base64(nil, false)
      break random_token unless ModelName.exists?(token: random_token)
    end
  end

  def destination_port_must_be_different_from_origin
    return unless self.origin_port_id == self.destination_port_id

    errors.add(:destination_port, I18n.t('quotes.errors.destination_port_must_be_different_from_origin'))
  end
end



```

Did you see the file above? This is a example of how a model class should be organizated. But to understand this organization, let's go to a deconstruction of this file.

### 1 - Modules

If you have any kind of module to import to this class, you can import this before any thing inside the class.

### 2 - Inheritance column name

The first thing you need to declare inside a model is the column you will use for inheritance. For example, if you have a model for users which have different kinds of *role* (admin, sub-admin, normal user), you can use this method to set the *type* of this user and because of this is the first thing declared, just to tell Rails what kind of record is.

### 3 - Devise kind methods

If you using some gem like devise which have methods specially made to import some modules, you can declare just after inheritance column name.

### 4 - Callbacks

The callbacks is the methods called when you interact with this class. For example, when you call a `.save` inside a controller, the first action which model classes do is call the callbacks.

### 5 - Relations

After the callbacks, we usually declare the relations of this model. Belongs before has_one/many.

### 6 - Validates and custom validates

The next thing to declare are the validations for this active record class. Looking the file above you can see that `validates` is called before `validate`. Why? Because the default validations like `presence`, `inclusion` ... should be called before custom validations like `destination_port_must_be_different_from_origin`. This is a good practice and you can see this around many active record classes in different projects around the GitHub.

### 7 - Enumerations

You can use any kind of gem or even the `enum` helpers added in Rails 4.0+.

### 8 - Uploaders

### 9 - Public Methods

Any kind of method which will be used and should be for public access.

### 10 - Protectec Methods

Protected methods should be used when you have things like a callback method which you want to use just for this object and for his inherited objects.

### 11 - Private methods

Usually used for custom validations, once this method should used just for this specific class.

## Contributing

1. Fork it ( https://github.com/codelandev/snippets/fork )
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request
