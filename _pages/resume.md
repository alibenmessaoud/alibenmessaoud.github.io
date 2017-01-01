---
layout: default
title: "Resume"
permalink: /resume/
---
***Last update from LinkedIn: 2021.02.01***

Education
---------
<ul>
	{% for school in site.data.profile.education %}
	<li>
		<b>
			{{ school.startDate | date: "%B %Y" }} - 
			{{ school.endDate | date: "%B %Y" }},
			{{ school.schoolName }}
		</b>
		<br />
		{{ school.degreeName }}, {{ school.fieldOfStudy }}
		<br />&nbsp;
 	</li>
	{% endfor %}
</ul>

Work Experience
---------------
<ul>
{% for experience in site.data.profile.experiences %}
	<li>
		<b>
			{{ experience.startDate | date: "%B %Y" }} - 
			{% if experience.endDateIsPresent == false %} {{ experience.endDate | date: "%B %Y" }} {% endif %}
			{% if experience.endDateIsPresent == true %} present {% endif %}
		</b>
            {%if experience.startDate != null and experience.startDate != ''
                   or experience.endDate != null and experience.endDate != '' 
            %} <br /> {% endif %}
		{{ experience.company }}, {{ experience.location.city }}, {{ experience.location.country }}
		    {%if experience.company != null and experience.company != ''
                   or experience.location.city != null and experience.location.city != '' 
                   or experience.location.country != null and experience.location.country != '' 
            %} <br /> {% endif %}
		{% assign experienceDescriptions = experience.description | replace: "… see more", "..." | split: "#" %}
		{{ experienceDescriptions[0] }}
		    {%if experienceDescriptions[0] != null and experienceDescriptions[0] != ''%} <br /> {% endif %}
		<i> {{ experienceDescriptions[1] }}  </i>
		    {%if experienceDescriptions[1] != null and experienceDescriptions[1] != ''%} <br /> {% endif %}

		<br />
  </li>
{% endfor %}
</ul>

Skills
---------
<ul>
	{% for skill in site.data.profile.skills %}
		<li>
			{{ skill.skillName }}
			{%if skill.endorsementCount > 0 %}
				({{ skill.endorsementCount }})
			{% endif %}
		</li>
	{% endfor %}
</ul>
