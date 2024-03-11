# AutoMapper example



{% code fullWidth="true" %}
```
// mapping time1, time2 (not null), ignore others (FirstTime, LastTime)

     CreateMap<SettingViewModel, Setting>()
      .IgnoreAllPropertiesWithAnInaccessibleSetter()
      .ForMember(dest => dest.FirstTime, o => o.Ignore())
      .ForMember(dest => dest.LastTime, o => o.Ignore())
      .ForAllMembers(opts => opts.Condition((src, dest, srcMember) => srcMember != null));
```
{% endcode %}

{% code fullWidth="true" %}
```
// Some code

private IMapper _mapper { get; set; }
private IPresentationService _viewModelServices { get; set; }

public AutomapperWrapper(IPresentationService viewModelServices, IWriteEntities _entities)
{
// TODO: need to cache categories, or at least lazy load
_viewModelServices = viewModelServices;

MapperConfiguration _mapperConfiguration = new MapperConfiguration(cfg =>
{
	cfg.CreateMap<Client, ClientDetailsViewModel>()
		.ForMember(dest => dest.ClientID, opt => opt.MapFrom(src => src.ID))
		.ForMember(dest => dest.SuburbDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSuburbSelectList()))
			.AfterMap((src, dest) =>
			{
				dest.Suburb = dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ResidentialSuburbID.ToString()) == null ?
				"" : dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ResidentialSuburbID.ToString()).Text;
			})
			.AfterMap((src, dest) =>
			{
				dest.ProvisionLocation = dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ProvisionLocationSuburbID.ToString()) == null ?
				"" : dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ProvisionLocationSuburbID.ToString()).Text;
			})
		.ForMember(dest => dest.GenderDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Gender")))
			.AfterMap((src, dest) =>
			{
				dest.Gender = dest.GenderDropDownList.SingleOrDefault(list => list.Value == src.GenderID.ToString()) == null ?
				"" : dest.GenderDropDownList.SingleOrDefault(list => list.Value == src.GenderID.ToString()).Text;
			})
		.ForMember(dest => dest.MarketingInfoList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryList("MarketingInfo")))
		.ForMember(dest => dest.PreviousMarketingInfo, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryListByUser("MarketingInfo", src.ID)))
	;
	cfg.CreateMap<Patient, PatientDetailsViewModel>()
		.ForMember(dest => dest.PatientID, opt => opt.MapFrom(src => src.ID))
		.ForMember(dest => dest.SuburbDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSuburbSelectList()))
			.AfterMap((src, dest) =>
			{
				dest.Suburb = dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ResidentialSuburbID.ToString()) == null ?
				"" : dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ResidentialSuburbID.ToString()).Text;
			})
			.AfterMap((src, dest) =>
			{
				dest.ProvisionLocation = dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ProvisionLocationSuburbID.ToString()) == null ?
				"" : dest.SuburbDropDownList.SingleOrDefault(list => list.Value == src.ProvisionLocationSuburbID.ToString()).Text;
			})
		.ForMember(dest => dest.GenderDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Gender")))
			.AfterMap((src, dest) =>
			{
				dest.Gender = dest.GenderDropDownList.SingleOrDefault(list => list.Value == src.GenderID.ToString()) == null ?
				"" : dest.GenderDropDownList.SingleOrDefault(list => list.Value == src.GenderID.ToString()).Text;
			})
		.ForMember(dest => dest.LanguageDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Language")))
			.AfterMap((src, dest) =>
			{
				dest.FirstLanguage = dest.LanguageDropDownList.SingleOrDefault(list => list.Value == src.FirstLanguageID.ToString()) == null ?
				"" : dest.LanguageDropDownList.SingleOrDefault(list => list.Value == src.FirstLanguageID.ToString()).Text;
			})
			.AfterMap((src, dest) =>
			{
				dest.SecondLanguage = dest.LanguageDropDownList.SingleOrDefault(list => list.Value == src.SecondLanguageID.ToString()) == null ?
				"" : dest.LanguageDropDownList.SingleOrDefault(list => list.Value == src.SecondLanguageID.ToString()).Text;
			})
		.ForMember(dest => dest.CultureDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Culture")))
			.AfterMap((src, dest) =>
			{
				dest.Culture = dest.CultureDropDownList.SingleOrDefault(list => list.Value == src.CultureID.ToString()) == null ?
				"" : dest.CultureDropDownList.SingleOrDefault(list => list.Value == src.CultureID.ToString()).Text;
			})
		.ForMember(dest => dest.ReligionDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Religion")))
			.AfterMap((src, dest) =>
			{
				dest.Religion = dest.ReligionDropDownList.SingleOrDefault(list => list.Value == src.ReligionID.ToString()) == null ?
				"" : dest.ReligionDropDownList.SingleOrDefault(list => list.Value == src.ReligionID.ToString()).Text;
			})
		.ForMember(dest => dest.CareTypeDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("CareType")))
			.AfterMap((src, dest) =>
			{
				dest.CareType = dest.CareTypeDropDownList.SingleOrDefault(list => list.Value == src.CareTypeID.ToString()) == null ?
				"" : dest.CareTypeDropDownList.SingleOrDefault(list => list.Value == src.CareTypeID.ToString()).Text;
			})
		.ForMember(dest => dest.RelationshipDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Relationship")))
		.ForMember(dest => dest.PetDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Pet")))
		.ForMember(dest => dest.InterestList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryList("Interest")))
		.ForMember(dest => dest.MedicalLivingNeedsList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryList("MedicalLivingNeed")))
		.ForMember(dest => dest.PreviousInterestList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryListByUser("Interest", src.ID)))
		.ForMember(dest => dest.PreviousMedicalLivingNeedsList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryListByUser("MedicalLivingNeed", src.ID)))
		.ForMember(dest => dest.Contacts, opt => opt.MapFrom(src => src.Contacts))
	;
	cfg.CreateMap<Shortlist, ShortlistViewModel>();

	cfg.CreateMap<ClientDetailsViewModel, Client>()
		.ForMember(dest => dest.Patients, opt => opt.Ignore())
		.ForMember(dest => dest.PostedJobs, opt => opt.Ignore())
		.ForMember(dest => dest.Shortlists, opt => opt.Ignore())
	;
	cfg.CreateMap<PatientDetailsViewModel, Patient>()
		.ForMember(dest => dest.Contacts, opt => opt.Ignore())
	;
	cfg.CreateMap<ShortlistViewModel, Shortlist>()
		.ForMember(dest => dest.ID, opt => opt.Ignore())
	;

	cfg.CreateMap<Contact, Contact>();
	cfg.CreateMap<List<Contact>, List<Contact>>();
	cfg.CreateMap<Shortlist, Shortlist>();

	cfg.CreateMap<Patient, PatientAttachmentViewModel>()
		.ForMember(dest => dest.PatientID, opt => opt.MapFrom(src => src.ID))
		.ForMember(dest => dest.AttachmentList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryList("Attachment")))
 		.ForMember(dest => dest.UploadFileSubCategoryIDs, opt => opt.MapFrom(src => _viewModelServices.GetSubCategoryListByUser("Attachment", src.ID)))
		.ForMember(dest => dest.PreviousAttachmentList, opt => opt.MapFrom(src => _viewModelServices.GetUserAttachmentList("Attachment", src.ID)))
		.ForMember(dest => dest.AttachmentUrlList, opt => opt.MapFrom(src => _viewModelServices.GetUserAttachmentUrlList(src.ID)))
	;


	cfg.CreateMap<Job, JobViewModel>()
	.ForMember(dest => dest.Id, opt => opt.MapFrom(src => src.ID))
	.ForMember(dest => dest.PatientId, opt => opt.MapFrom(src => src.PatientId))
	.ForMember(dest => dest.ServicedAt, opt => opt.MapFrom(src => src.ServiceAt))
	.ForMember(dest => dest.SuburbDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSuburbSelectList()))
	.ForMember(dest => dest.GenderDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("Gender")))
	.ForMember(dest => dest.ServiceDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetSubCategorySelectList("JobService")))
	.ForMember(dest => dest.PatientDropDownList, opt => opt.MapFrom(src => _viewModelServices.GetPatientSelectList(src.ClientId)))
	;

	cfg.CreateMap<JobViewModel, Job>()
	.ForMember(dest => dest.ID, opt => opt.MapFrom(src => src.Id))
  .ForMember(dest => dest.PatientId, opt => opt.MapFrom(src => src.PatientId))
	.ForMember(dest => dest.ServiceAt, opt => opt.MapFrom(src => src.ServicedAt))
	;

});
_mapper = new Mapper(_mapperConfiguration);
}

```
{% endcode %}
