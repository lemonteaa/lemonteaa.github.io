---
layout: post
title:  "The Monad Tutorial Fallacy, Part Two: Monad, Kleisli and Eilenberg-Moore Category"
date:   2017-10-03 20:36:00 +0800
categories: fundamentals
comments: true
math: true
---
*(Go to [Part 1][MonadPartOne] of this series)*

In part 2 of this series we explain the technical core of the theory of Monad. Of central importance is the attempt to provide algebraic interpretation of everything, as well as to convert things into suitably algebraic structures. We will also revisit this issue at an arguably "better" (but more advanced) angle in part 5.

While the picture for an adjunction is relatively clear, it suffers from needing to work with two categories at the same time. Monad fixes this by composing the left and right adjoint functors into an endofunctor $$T = G \circ F$$, $$T: \mathcal{C} \rightarrow \mathcal{C}$$ over a single category. An additional advantage with this construction is that it is more algebraic as it is now composable: we can iterate $$T$$ to get $$T^2$$, $$T^3$$ etc for example. [^1]
<!--more-->

## On the benefit of being Algebraic/Functional
It is perhaps appropriate at this point to pontificate on what does it means to be "algebraic", and why do we obsesses over it, before proceeding further on the details. From my armchair philosophical perspective, being "algebraic" simply mean having a system of rules for manipulating symbols. But this "definition" is obviously wrong, for with this requirement the whole of pure mathematics at the very least qualifies! (All math are built from logic, and formal logic is just such a system of rules, albeit a bit more complicated than arithmetic) What distinguishes algebra from other such formal system is an intrinsic quality that it is easy and convenient to use.

But "easy" is subjective. And there seems to be more than one kind of feature that contribute to this elusive quality. These features include:

* The rules and system should be as Context-free as possible, so that operations can be liberally applied, and so that a piece of operation/reasoning carried out somewhere can be taken and re-applied anywhere else
  - i.e. Equational Reasoning
  - And in other words things should be reusable.
* Composability. (Excuse my circular reasoning) It should be possible to chain functions together and if these functions are expressed with certain specific structures in mind, these structures should be preserved after composition.
  - Moreover, it should ideally have the Compositionality property - the meaning of a composed function should be derivable from the meaning of the individual functions alone.
  - What they imply are that you have an easy way to build larger structures out of smaller one, while controlling (semantic) complexity.

If you are a programmer, you may find this list suspiciously familiar even though you may have forgotten all the math since you're done with high school. Basically they are just the benefits of functional programming: pure functions are composable, safe, reusable, and easy to reason about and understand.

## Defining Monad is a Dangerous Affair
If one dig around a bit in Haskell's community, one can easily see how this is a topic laden with traps, controversies, and a sea of confused/perplexed beginners. There is an active advise against posting new Monad tutorials. There is a trail of [past attempts](https://wiki.haskell.org/Monad_tutorials_timeline) at such tutorials. There is a post in the wiki specifically to debunk what [Monad is not](https://wiki.haskell.org/What_a_Monad_is_not) and notwithstanding such effort, it is (in this author's opinion) next to impossible to eradicate those "wrong" definitions, not even in principle. Oh and if you try to read Monad tutorial (like this one), you will find out that everyone's definition is [different](http://www.codecommit.com/blog/ruby/monads-are-not-metaphors). [^2]

That's quite a downer, to put it mildly. Why? [^3]

*\<begin rant\>*

Please bear with me as I do a second round of philosophizing and go meta. Defining real world, nebulous concepts is almost always messy (in contrast to the pristine one you get in pure math). They are typically complicated. Have multiple facets. Have multiple possible perspectives to look at that are mostly equally reasonable within their own frameworks. (Or, at least, no one managed to convince anyone else in case of a conflict) Have multiple layers of understanding possible to attain. Ad Nauseum.

With this kind of stuff, saying any particular definition is "wrong" is meaningless without defining "wrongness" within a suitable context. Say in a hypothetical (i.e. oversimplified) situation there is a hierarchy of definitions possible, from the most specific to the most general. Is the specific one wrong because it fails to capture the full range of possibilities, or the general one wrong because it is utterly useless for application? Sometimes I think we geeks and nerds need to stop pretending math/logic/programming is the solo, absolute standard of truth in all domains. Just go read some (ok, any) sociology texts and resists the temptation to call everything in it bullshit. (except some toxic part of postmodernism maybe?)

*\<end rant\>*

But it seems irresponsible to go on without any definition at all besides the formal one. So, in light of the sensitiveness of this issue, I will tread carefully and give more than one definitions.

A Monad is not about statefulness/impurity. It's also not about explicit sequencing of execution. They are, however, possible applications of Monad. (Among others)

### Most general definition I can think of
(Inspired by this [brilliant comment](https://www.reddit.com/r/haskell/comments/68fb19/monad_tutorial_no_57005/dgynjik/) over there)

> Monad is a mathematical theory that can be used to extend the semantics of a programming language in a principled manner, provided the extra structures admits an algebraic formulation.

Because of this I believe the closest comparison one can make is versus Macro in the Lisp family of language [^4]. At least they are roughly at the same conceptual level of abstraction.

One should also be careful to distinguish between Monad-as-a-concept, versus how it is implemented in actual languages.

Monad is not a [silver bullet](http://lambda-diode.com/programming/monads-are-a-class-of-hard-drugs) nor black magic. It is, in the final analysis, up to you to decide what to do with it.

### Operational definition - Full buy in
> A Monad is a contract between the programmer and the language. Programmer agrees to structure their program in a particular form, and to supply proof of the Monad axiom themselves, outside of the program. In return, the language promises automatic guarantee of some properties of the program.

But asking a programmer to write proof is scary, horrible stuff. Monad is thoughtful in this regard:
As there is multiple equivalent formulation of Monad, one have the option to choose which one to work on.
The math statements that one is actually required to prove is also algebraic/equational in nature, so it is in principle possible to just compute/derive with lambda calculus blindly and hope that the equations just line up, as opposed to requiring professional grade mathematical literacy.

### Operational definition - Partial buy in
In case even that is too much, it is also possible to write code in a monadic style. In this case one adopts patterns inspired by the Monad, but refrain from (actively) doing the proof or making their code completely compliant to the requirement. One may still gain some benefits, but foregoes the mathematical guarantee offered by the theory.

## Formal definition of Monad
For delicate, highly abstract entity like Monad (as well as other heavily categorical construct, and especially things involving [higher category](https://ncatlab.org/nlab/show/higher+category+theory)), one can argue that the only safe way to grok it would be to look at the full, formal definition (otherwise one runs the risk of missing a "small" piece of the definition - which usually contains a "large" number of such pieces - that turns out to be crucial in unexpected place). But doing so come with the cost of things being opaque and complicated. While I may not be able to solve the "complicated" part (irreducible complexity, say), I have tried to deal with the first by presenting a mixture of motivation, discourse, supporting theories, etc.

Even so, we won't try to understand Monad all in one go, preferring an iterative and indirect approach instead. We will provide a preliminary, algebraic understanding of Monad, only in the context of classical Mathematics (through the "Universal" example of Free algebra vs Forgetful Functor). In part 3 of this series we will give an interpretation within the context of raw vs structured computational model, alongside actual examples of Monad in programming, while in part 4 we will give alternative, equivalent formulation of Monad that admits an easier interpretation in the computational context. (Part 5's purpose has been stated at the beginning of this article) Nonetheless, all these niceties depends on the core theory presented in this part.

The actual definition of [Monad](https://en.wikipedia.org/wiki/Monad_(category_theory)) is here (Don't worry if you don't get it all at once! The important thing in this part is actually the theory of Kleisli and Eilenberg-Moore Category since I don't see them fully interpreted anywhere except for some handwaving. Just read on and stay tuned for part 3 - 5 :P ):

* A Monad over a category $$\mathcal{C}$$ is a triple $$(T, \eta, \mu)$$ where $$T$$ is a functor $$\mathcal{C} \rightarrow \mathcal{C}$$.
  - (for those not studying math: I must highlight that a functor when implemented in a programming language is actually two things: $$T$$ here maps objects in $$\mathcal{C}$$ to objects in $$\mathcal{C}$$, and **also** morphisms/function in $$\mathcal{C}$$ to morphisms/function in $$\mathcal{C}$$, where the (co)domain object is also mapped. That is, $$T$$ sends $$f: X \rightarrow Y$$ into $$Tf: TX \rightarrow TY$$)
* $$\eta$$ is the unit (sometimes called `return` in programming), which is a natural transformation from the identity functor to $$\mathcal{C}$$. More concretely for any object $$X$$ in $$\mathcal{C}$$, we have a morphism/function $$\eta_X: X \rightarrow TX$$.
* And $$\mu$$ is the "multiplication" / "algebraic operation" (called `join` or `flatten` in programming). It is also a natural transformation, this time from $$T^2$$ to $$T$$. That is it is a polymorphic function from $$T^2 X$$ to $$TX$$.
  - Notice that both [functor](https://en.wikipedia.org/wiki/Functor#Definition) and [natural transformation](https://en.wikipedia.org/wiki/Natural_transformation#Definition) obey certain constraints/equations/axioms, which are not listed here.
* Finally, our $$\eta$$ and $$\mu$$ satisfies two additional coherence conditions, expressed via commutative diagrams:

$$
\array{
    \phantom{\mu T} T^3 & \stackrel{T \mu}\longrightarrow & T^2 \\
    ^{\mu T}\downarrow  &                                 & \downarrow^\mu \\
    \phantom{\mu T} T^2 & \stackrel{\mu}\longrightarrow   & T
}
\qquad
\array{
	\phantom{T \eta} T   & \stackrel{\eta T}\longrightarrow & T^2 \\
	^{T \eta} \downarrow & \searrow^\text{id}               & \downarrow^\mu \\
	\phantom{T \eta} T^2 & \stackrel{\mu}\longrightarrow    & T
}
$$

As many people/articles/tutorials have observed, these laws look a lots like the identity and associativity law of an algebra (say a group):
* (Identity) For all $$x$$, $$0 + x = x + 0 = x$$.
* (Associativity) For all $$x, y, z$$, $$x + (y + z) = (x + y) + z$$.

We will in time comes to a fuller understanding of this point, through multiple rounds of seeing this play out in different level of abstractions.

Before moving on, here's a technical aside (included since we'll assume knowledge of this bit when doing proofs later on in this post): the notation mixing functor and natural transform above is a shorthand. Suppose $$\epsilon: F \Rightarrow G$$ is a natural transform from $$F$$ to $$G$$, and let $$H$$ be another functor. Then $$H\epsilon$$ means applying the functor $$H$$ to the arrow $$FX \stackrel{\epsilon_X}\longrightarrow GX$$ to get $$H(FX) \stackrel{H \epsilon_X}\longrightarrow H(GX)$$. Meanwhile, $$\epsilon H$$ actually means the natural transform applied at the object $$HX$$ instead of $$X$$, i.e. $$F(HX) \stackrel{\epsilon_{HX}}\longrightarrow G(HX)$$.

We still need to connect this formal definition to our earlier motivation of composing adjoint functors. In fact there is a general construction to get a monad from any adjoint functors. $$T$$ is just $$G \circ F$$ as mentioned before. The unit is just the unit in the adjunction. The join operation is obtained by applying the counit to $$T^2$$, that is: $$T^2 = (GF)(GF) = G(FG)F \longrightarrow G \operatorname{1}_{\mathcal{D}} F = GF = T$$. (While this general construction works, I can't get a satisfactory interpretation in the context of computational model. So I'll take a detour and interpret a different, but equivalent formulation of Monad in part 4)

Now let's interpret this using the example of free algebra vs forgetful functors. $$T$$ simply map any set $$X$$ to the underlying set of its free algebra $$F(X)$$, and maps function between set to the naturally induced map between their corresponding free algebra, as usual. The unit provides the embedding of the original set $$X$$ into the free algebra('s underlying set). The really non-trivial part is the join operation. Notice that $$T^2X$$ is a set consisting of algebraic expression of algebraic expression over $$X$$. If this is befuddling, we can also say that it is an algebraic expression where one layer of variable definition is allowed. For example:
If $$X = \{ x, y, z \}$$, then let $$p = 2x+1, q=x-y+3z, r=y^2 + xz$$, and let the final expression be $$p^2 - q + 2r$$.

Now there is an obvious way to simplify this: just (partially) evaluate it by substitution: $$(2x+1)^2 - (x-y+3z) +2(y^2 + xz) = \ldots $$

We can see how this is related to the general construction of the join operation: Recalling that an expression evaluator is possible in any algebra (and given by the counit), a substitution evaluator is really just a plain evaluator over the free algebra $$F(X)$$, so we have $$FGF(X) \longrightarrow F(X)$$. Now simply apply forgetful functor on both sides.

## Kleisli Category
We use monad in practise instead of adjunction when programming because it is difficult to specify an entirely new computational model, while it is "easy" (in the sense of not having to leave your current programming environment) to implement things within the current computational model. While our conceptual picture is that of constructing/deriving a monad from an adjunction, we can still ask the converse question: does every monad arise from/implies an adjunction?

Happily, it turns out the answer is a resounding "Yes". In fact there is a family of possible adjunctions that compose to the same monad. Even more luckily, there is both a minimal and maximal such adjunctions, and they are the minimal, [Kleisli category](https://en.wikipedia.org/wiki/Kleisli_category) consisting of just the free algebra, versus the maximal, [Eilenberg-Moore category](https://en.wikipedia.org/wiki/F-algebra) of all algebra. We will look at both in turns.

Suppose that we just assume the existence of any such adjunction, without knowing what the other category $$\mathcal{D}$$ is (nor the individual functors $$F$$ or $$G$$). Further assume that they are really just some form of Free algebra/Forgetful functors. At the very least, $$\mathcal{D}$$ must contain the image of $$F$$ over $$\mathcal{C}$$, which are all the free algebras. Since we don't really know what $$F$$ is, we will just let any object $$X$$ in $$\mathcal{C}$$ stands for an unknown object $$F(X)$$. [^5] Hence we can let the set of objects in $$\mathcal{D}_{\text{min}}$$ be just all the objects in $$\mathcal{C}$$.

Now how about the morphisms? *Prima facie*, they should really be just the algebra's homomorphisms, i.e. $${\operatorname{Hom}}_{\mathbf{Alg}}(FX, FY)$$, and composition of morphism is just ordinary composition of function. However we can't do that since we don't directly know $$F$$. Instead we need to do so indirectly by expressing them in terms of things we have direct access to only. So appealing to the Hom-adjunction property of an adjunction, we have that that set is naturally isomorphic to $${\operatorname{Hom}}_{\mathbf{Set}}(X, UFY) = {\operatorname{Hom}}_{\mathbf{Set}}(X, TY)$$, and we can just let this latter set be the definition of morphisms between $$X$$ and $$Y$$ in $$\mathcal{D}$$.

A challenge that remains is to define composition of morphisms. Per the reasoning/interpretation for the Hom-Set adjunction we talked about in [Part 1][MonadPartOne], given an arrow $$X \longrightarrow TY$$ it should be possible to uniquely extend it into a morphism $$TX \longrightarrow TY$$. Then we can just compose as usual: $$X \longrightarrow TY \longrightarrow TZ$$ is an arrow from $$X$$ to $$TZ$$, which fits the representation of morphism in $$\mathcal{D}_{\text{min}}$$. In programming this extension operation is called a `bind` (After switching order of arguments if necessary and using the curried form). [^6]

It turns out that bind can be expressed in terms of a Monad by sending $$X \stackrel{\phi}\longrightarrow TY$$ into $$TX \stackrel{T\phi}\longrightarrow T^2 Y \stackrel{\mu_Y}\longrightarrow TY$$. We can prove that this is correct, namely, that the result of extension defined this way, is the same as applying the forgetful functor to the same morphism $$\phi$$ represented as an honest algebra homomorphism $${\operatorname{Hom}}_{\mathbf{Alg}}(FX, FY)$$.

First note that the extended morphism has at least one $$T$$ throughout ( $$UFX \stackrel{UF\phi}\longrightarrow (UF)(UF)Y \stackrel{\mu_Y}\longrightarrow UFY$$), so we can strip the outer $$U$$ (due to functor's law), and then it suffices to show that the morphism $$FX \stackrel{F\phi}\longrightarrow FUFY \stackrel{\eta_{FY}}\longrightarrow FY$$ is $$\phi$$ mapped by the natural isomophism between $${\operatorname{Hom}}_{\mathbf{Set}}(X, UFY)$$ and $${\operatorname{Hom}}_{\mathbf{Alg}}(FX, FY)$$. Now note that by category theory, the counit $$\eta_{FY}$$ is really just the identity on $$UFY$$ mapped through the natural isomorphism, and that by naturality's commutative diagram, pre-composing it with $$F\phi$$ is then the same as just pre-composing $$\text{id}_{UFY}$$ with $$\phi$$ (which is then still $$\phi$$), and then applying the natural isomorphism, and so we are done.

If this is too dense, a rough sketch of the proof is that the counit (expression evaluator) extend to the identity, and that since extension is basically just the natural isomophism of an adjunction, we can indirectly get what we want by applying the counit and relying on the "commutative" property of extension.

For the case of actual free algebra, what this operation amount to is to first induce a map from the free algebras generated by $$X$$ and $$TY$$ by considering $$\phi$$ as just an honest map between sets (ignoring the structures in $$TY$$), and then collapse the nested algebraic expression that results by substitution.

Before moving on we want to check that composition defined this way is associative. Since the composition is built out of the extension operator, we only need to check the following:
$$(g^* \circ f)^* = g^* \circ f^*$$ (Which can be seen as an indirect form of idempotency. It cannot be stated directly because the extension operator cannot be applied to the same function twice)

We compute

$$\begin{eqnarray} 
(g^* \circ f)^* &=& (X \stackrel{f}\longrightarrow TY \stackrel{g^*}\longrightarrow TZ)^* \\
&=& T \left( X \stackrel{f}\longrightarrow TY \stackrel{g^*}\longrightarrow TZ \right) \stackrel{\mu_Z}\longrightarrow TZ \\
&=& TX \stackrel{Tf}\longrightarrow T^2 Y \stackrel{Tg^*}\longrightarrow T^2 Z \stackrel{\mu_Z}\longrightarrow TZ
\end{eqnarray}$$

Now since $$g^*$$ is $$TY \stackrel{Tg}\longrightarrow T^2 Z \stackrel{\mu_Z}\longrightarrow TZ$$, $$Tg^*$$ is $$T^2 Y \stackrel{T^2 g}\longrightarrow T^3 Z \stackrel{T \mu_Z}\longrightarrow T^2 Z$$.

We can use the associativity of $$\mu$$, and then the naturality of $$\mu$$, to change to

$$\begin{eqnarray}
&& TX \stackrel{Tf}\longrightarrow \left( T^2 Y \stackrel{T^2 g}\longrightarrow T^3 Z \stackrel{T \mu_Z}\longrightarrow T^2 Z \right) \stackrel{\mu_Z}\longrightarrow TZ \\
&=& TX \stackrel{Tf}\longrightarrow T^2 Y \stackrel{T^2 g}\longrightarrow \left( T^3 Z \stackrel{\mu_{TZ}}\longrightarrow T^2 Z \stackrel{\mu_Z}\longrightarrow TZ \right) \\
&=& TX \stackrel{Tf}\longrightarrow \left( T^2 Y \stackrel{\mu_Y}\longrightarrow T Y \stackrel{Tg}\longrightarrow T^2 Z \right) \stackrel{\mu_Z}\longrightarrow TZ \\
&=& g^* \circ f^*
\end{eqnarray}$$

as desired.

We can also reconstruct $$F$$ and $$G$$ from the monad $$T$$ using Kleisli Category, and it is mostly common sense. $$F$$ should just send any object in $$\mathcal{C}$$ to the same object in $$\mathcal{D}_{\text{min}}$$ (due to the way we relabel things). Morphisms in $$\mathcal{C}$$ can be embedded in $$\mathcal{D}_{\text{min}}$$ by just composing with the unit, aka $$X \stackrel{f}\longrightarrow Y \stackrel{\mu_Y}\longrightarrow TY$$. (Check that this is a functor!) Similarly, to forget things, $$X$$ in $$\mathcal{D}_{\text{min}}$$, which represent $$F(X)$$, should be sent to $$UF(X) = TX$$. Morphisms in $$\mathcal{D}_{\text{min}}$$, say $$X \stackrel{g}\longrightarrow TY$$, will need to sent to some function of type $$TX \longrightarrow TY$$. So we just apply the extension operator since it is unique anyway.

## Eilenberg-Moore Category
At the opposite extreme, suppose we want to construct the category of all "algebra". The challenge here is that we need to specify it without referring to anything internal to the objects itself (say, the addition operation in abelian groups), since for the construct to be general we must not make any assumption on what kinds of objects they are actually. Instead we can try to appeal to the interrelationship between objects. This kind of approach to generalizing things beyond their original context/example is known as "categorification" in mathematics.

So an algebra is just a set with all the algebraic operation specified over that set. Per the remarks above we are not allowed to refer to the actual algebra, although we can refer to the underlying sets. (As they are in category $$\mathcal{C}$$) Recalling what we said in [part 1][MonadPartOne], a cunning trick to recover the algebra is to specify an expression evaluator instead. (This works because expression encompass and generalize algebraic operation. For example, to fully specify $$+$$, it suffices to know the value of the expression $$a + b$$ where $$a$$ and $$b$$ can range over everything in $$X$$) This we can do: objects in our category $$\mathcal{D}_\text{max}$$ is just an arrow $$TX \stackrel{\alpha}\longrightarrow X$$, where $$X$$ is in $$\mathcal{C}$$. Here $$X$$ is the underlying set, $$TX$$ is a free algebra whose elements are all the expressions, and the morphism is then the evaluator (it maps expression over $$X$$ to value in $$X$$ again).

However, the formulation here is too loose: we must somehow ensure that the evaluator is valid, that it obey the algebra's laws/axioms. Part of this work is already done by the free algebra construct hidden within $$T$$, since expressions that are provably identical using the axioms of the algebra are quotiented out (i.e. identified) in $$F(X)$$. The remaining part is a baggage brought on by the fact that $$TX$$, as a free algebra, has extra structures not present when we are just talking about the value of $$a + b$$ (with $$a$$ and $$b$$ mere values and not expressions).

To do this we appeal to the monad, again applying the idea that expression generalize algebraic operation. Namely, we require the following diagram commutes:

$$
\array{
	T^2 X            & \stackrel{T \alpha}\longrightarrow & TX \\
	\downarrow^{\mu} &                                    & \downarrow^{\alpha} \\
	TX               & \stackrel{\alpha}\longrightarrow   & X
}
\qquad
\array{
	X & \stackrel{\eta}\rightarrow  & TX \\
	  & {}_{\text{id}} \searrow     & \downarrow^{\alpha} \\
	  &                             & X
}
$$

What does it mean? Suppose we have an expression of expression. The two ways to evaluate it would be to either substitute then evaluate (monad join, followed by $$\alpha$$), or evaluate twice - evaluate the value of intermediate variables first, then evaluate the resulting expression. ($$T \alpha$$ then $$\alpha$$) They should obviously be the same. As an example, let $$u = x + 2y$$ and $$v = y - z$$. Suppose that $$x = 3, y = 7, z = 1$$. Then $$u = 17, v = 6$$ and so $$u + v = 23$$. On the other hand $$u + v = x + 3y - z$$, so the evaluator have to map $$x + 3y - z$$ to $$23$$ (as this is not otherwise mandated since this expression is different from either $$u$$ or $$v$$, and $$TX \longrightarrow X$$ is just a map from set to set).

With the basic structure out of the way, specifying homomorphisms between algebra is actually pretty easy. Consider that a hom is a function between the underlying set which must also respect the algebraic structure. In ordinary algebra this is expressed through commutative diagrams for each algebraic operations. For instance:

$$
\require{AMScd}
\begin{CD}
\stackrel{(a, b)}{G \times G}             @>+>> \stackrel{(a + b)}{G} \\
@V{(\phi, \phi)}VV                              @VV{\phi}V \\
\underset{(\phi(a), \phi(b))}{H \times H} @>+>> \underset{\phi(a + b) = \phi(a) + \phi(b)}{H}
\end{CD}
$$

Because the structure of each algebra is already constrained by the requirement above, we can just naively generalize this diagram, turning the artificial $$G \times G$$ into the more general set of expression $$TX$$:

$$
\require{AMScd}
\begin{CD}
T(X)     @>{\alpha}>> X \\
@V{Tf}VV              @VVfV \\
T(Y)     @>{\beta}>>  Y
\end{CD}
$$

It remains to show that this does indeed form a category, and the main work to do is to check the composition of morphisms. Well, we can just stack the commutative diagram together:

$$
\require{AMScd}
\begin{CD}
T(X)     @>{\alpha}>> X \\
@V{Tf}VV              @VVfV \\
T(Y)     @>{\beta}>>  Y \\
@V{Tg}VV              @VVgV \\
T(Z)     @>{\gamma}>>  Z \\
\end{CD}
$$

Recalling that our goal is to prove the outer square commutes, we can perform diagram chasing: first note that the arrows $$X \stackrel{f}\longrightarrow Y \stackrel{g}\longrightarrow Z$$ is the same as $$X \stackrel{g \circ f}\longrightarrow Z$$, and by functor's axiom we can apply $$T$$ to this "equation" to get that the arrow on the left hand side can also be decomposed: $$X \stackrel{T(g \circ f)}\longrightarrow Z$$ is the same as $$X \stackrel{Tf}\longrightarrow Y \stackrel{Tg}\longrightarrow Z$$. Then just apply the commutativity condition for the two inner squares to transform:

$$
(g \circ f) \circ \alpha = g \circ (f \circ \alpha) = g \circ \beta \circ Tf = \gamma \circ Tg \circ Tf = \gamma \circ T(g \circ f)
$$

## Full Circle Back
Finally, as free algebra are also algebra, we want to see how the Kleisli category embed into the Eilenberg Moore Category. Well, the underlying set of a free algebra generated by $$X$$ is just $$TX$$, so we need an arrow $$T^2 X \longrightarrow TX$$. The monad join fits the bill here (and not just because the signature match[^7]) Why? Because algebraic operations in a free algebra is in fact defined by the substitution semantics. Anyway we still need to verify the conditions/diagrams for being an algebra. The first diagram is commutative because $$\mu$$ is a natural transform. The second diagram is just the unit law of monad.

How about morphism? We need a representation that is of type $$TX \longrightarrow TY$$ - which means for a morphism $$\phi: X \longrightarrow TY$$ we should use its extension $$\phi^*$$. Then we should check that it is in fact a morphism in the Eilenberg-Moore Category:

$$
\array{
	T^2 X                 & \stackrel{\mu}\longrightarrow   & TX \\
	\downarrow^{T^2 \phi} &                                 & \downarrow^{T \phi} \\
	T^3 Y                 & \stackrel{\mu T}{\cdots\cdots}  & T^2 Y \\
	\downarrow^{T \mu}    &                                 & \downarrow^{\mu} \\
	T^2 Y                 & \stackrel{\mu}\longrightarrow   & TY
}
$$

The diagram constructed from $$\phi^*$$ is shown above (with the obvious step for decomposing $$T(\phi^*)$$ omitted as it is the same as what we've seen before - just use the functor's law). To prove that it commutes, connect the middle row with the morphism $$\mu T$$. Then the bottom square commutes by the multiplication law of monad, while the upper square commutes due to $$\mu$$ being a natural transform. Then we're done by performing the usual diagram chasing.

Incidentally, this provide an alternative proof that the extension operator is correct: as extension is unique, we only need to ensure that:

1. The extended morphism remains the same on $$X$$
2. It is a homomorphism on $$\mathcal{D}$$ and not just $$\mathcal{C}$$.

The argument above amount to showing 2. For 1, we precompose with the unit:
$$\begin{eqnarray}
&&X \stackrel{\eta}\longrightarrow TX \stackrel{T \phi}\longrightarrow T^2 Y \stackrel{\mu}\longrightarrow TY \\
&=& X \stackrel{\phi}\longrightarrow TY \stackrel{\eta}\longrightarrow T^2 Y \stackrel{\mu}\longrightarrow TY \text{(unit is natural)} \\
&=& X \stackrel{\phi}\longrightarrow TY.
\end{eqnarray}
$$

## One more thing...

Right when I'm doing the final editing I come across [this](http://blog.sigfpe.com/2009/12/where-do-monads-come-from.html) and at a glance the approach it took looks similar to what I did here (free algebra/algebraic expression, that kind of stuff), although I took a pure math approach in case you're too thick :P.

*(Monad Series: To be continued...)*

----

[^1]: This does not mean that the adjunction construct is not algebraic - in fact a Category can be thought of as a groupoid, where elements can be multiplied provided their domain/co-domain match. Then an ordinary group is just a groupoid with only one domain so that anything can be multiplied with anything.

[^2]: But I can't resist the temptation to include a Matrix quote:
    > The Matrix is everywhere. It is all around us. Even now, in this very room. You can see it when you look out your window or when you turn on your television. You can feel it when you go to work... when you go to church... when you pay your taxes. It is the world that has been pulled over your eyes to blind you from the truth.
> 
> ...
> 
> Unfortunately, no one can be…told what the Matrix is. You have to see it for yourself.
> 
> Morpheus, The Matrix (1999)

[^3]: But fear not, for the Haskell community recognized this problem long ago (How can they not when you have endless waves of people asking about Monad?), and I think somewhere, nice and smart people are trying to find a better way. For example, see [this presentation](https://www.slideshare.net/linecorp/the-monad-fear).

[^4]: But Haskell also has something similar - template. See [this](https://www.reddit.com/r/haskell/comments/61r64w/what_does_the_free_monad_offer_that_macros_dont/) for an advocacy.

[^5]: Those not coming from a math/abstract algebra background may find this strange. Rest assured it is a culturally standard practise in math. One of the very first thing you learn in university algebra is that the actual name/label given to things in a set doesn't matter - so long as you always get the right things when called upon to. (In fact recall that the definition of a Category doesn't really require looking into the *content* of an object) So using $$X$$ instead of the actual $$F(X)$$ doesn't hurt - given such a label we can get back the real things, metaphorically, by applying the functor $$F$$.

[^6]: The naming here may look strange. It will look much more "natural" (no pun intended) when doing actual programming where we care about actually applying those functions.

[^7]: A gripe I have with the statically typed camp of programming, even for the sufficiently-advanced-type-that-offer-automatic-safety-guarantee, is that placing too much focus on the type risks crowding out my cognitive capacity to understand the actual semantics of a piece of code. It takes discipline to resist the temptation to consider a function correct just because the signature match.

[MonadPartOne]: {{ site.baseurl }}{% post_url 2017-08-28-the-monad-tutorial-fallacy-part-one-introduction-prequel %}
